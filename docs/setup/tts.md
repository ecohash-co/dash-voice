# Text-to-Speech Setup

DashVoice supports three TTS backends. They differ in quality, latency, and where the audio gets synthesized — there's no single "best" choice, so this guide gives you the trade-offs.

| Backend | Where it runs | Quality | Latency | Cost | Best for |
|---|---|---|---|---|---|
| **HA Piper** | Inside Home Assistant | Good | Low | Free | First-time setup, low-spec HA boxes |
| **Kokoro via speaches** | A CPU on your network | Great | Low | Free | The best free option if you have a spare machine |
| **OpenAI TTS** | OpenAI's cloud | Excellent | Medium | ~$0.015/1k chars | Premium voices, zero local setup |

You can change backends anytime in **Settings → Voice → TTS**. DashVoice fetches available voices from whichever backend you pick, so the voice picker stays in sync.

## HA Piper (default)

If you've set up Home Assistant Voice or the Assist Pipeline, you already have Piper running. DashVoice just uses HA's TTS service.

**Configuration**
- TTS Mode: `HA Piper`
- Voice: pick from the list (auto-populated from HA)

That's it. No extra service to run.

**Common voices**
- `en_US-ryan-low` — masculine, low-latency, the safest default
- `en_US-lessac-medium` — feminine, slightly heavier
- `en_GB-jenny_dioco-medium` — British female

Piper voices are pre-trained — quality is good but not great. If you find them robotic, try Kokoro.

## Kokoro via speaches

[speaches](https://github.com/speaches-ai/speaches) is an OpenAI-compatible TTS server that hosts the [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M) model. The output is notably better than Piper — closer to commercial cloud voices — and it still runs on CPU faster than real-time.

### Step 1 — Run speaches

```yaml
# docker-compose.yml
services:
  speaches:
    image: ghcr.io/speaches-ai/speaches:latest-cpu   # or :latest-cuda for GPU
    container_name: speaches
    restart: unless-stopped
    ports:
      - "9000:8000"
    environment:
      - ENABLE_UI=true
    volumes:
      - ./speaches/hf-cache:/home/ubuntu/.cache/huggingface
```

```bash
docker compose up -d speaches
```

First start downloads the Kokoro model (~330 MB). Once it's settled, test (replace `<speaches-host>`):

```bash
curl -s "http://<speaches-host>:9000/v1/audio/speech" -X POST \
  -H "Content-Type: application/json" \
  -d '{"model": "speaches-ai/Kokoro-82M-v1.0-ONNX", "input": "Hello world", "voice": "bf_emma"}' \
  --output test.mp3
```

If `test.mp3` plays back as natural speech, you're done.

### Step 2 — Configure DashVoice

| Field | Value |
|---|---|
| TTS Mode | `OpenAI-compatible` |
| Endpoint | `http://<speaches-host>:9000` |
| Model | `speaches-ai/Kokoro-82M-v1.0-ONNX` |
| Voice | `bf_emma` (or your pick) |
| Speed | 1.0 (default; range 0.5–2.0) |

From the HTTP API (replace `<tablet-ip>`, `<api-password>`, `<speaches-host>`):

```bash
TABLET=<tablet-ip>
PASSWORD=<api-password>

curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=ttsMode&value=openai"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=ttsEndpoint&value=http://<speaches-host>:9000"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=ttsModel&value=speaches-ai/Kokoro-82M-v1.0-ONNX"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=ttsVoice&value=bf_emma"
```

### Voice picker reference

Kokoro voices follow a `<lang><gender>_name` convention:

| Voice | Description |
|---|---|
| `bf_emma` | British female — warm, friendly (a popular default) |
| `bm_george` | British male |
| `af_heart` | American female — newsreader-ish |
| `af_bella` | American female — softer |
| `am_michael` | American male — neutral |
| `am_adam` | American male — deeper |

The full list is at the speaches `/v1/audio/voices` endpoint; DashVoice auto-fetches and groups by language.

## OpenAI TTS (cloud)

If you'd rather pay $0.015/1k characters for excellent voices and zero local setup, point DashVoice at the OpenAI API directly.

| Field | Value |
|---|---|
| TTS Mode | `OpenAI-compatible` |
| Endpoint | `https://api.openai.com` |
| API Key | your `sk-...` key |
| Model | `tts-1` (fast) or `tts-1-hd` (higher quality) |
| Voice | `alloy`, `echo`, `fable`, `onyx`, `nova`, or `shimmer` |

Latency is ~500 ms for short utterances, longer over slow connections. Bills hit your OpenAI account.

DashVoice does **not** send transcripts to OpenAI — only the response text from your Home Assistant conversation agent. If you're using a self-hosted conversation agent, none of your voice commands leave your network even with cloud TTS.

## Troubleshooting

**TTS too quiet**
- Most often a per-app volume issue. Bump the media volume on the tablet — DashVoice plays TTS on the media (`STREAM_MUSIC`) channel.
- If you're on HA Piper, try `OpenAI-compatible` with speaches; Piper's output level varies by voice.

**TTS sounds clipped or stuttery**
- Network jitter — DashVoice streams the audio from the TTS server as it's generated. Switch to a wired/closer network or move the TTS server closer.
- Lower the speed setting to 0.9× to give the network more headroom.

**"Hey Jarvis, what time is it" gets no audio response**
- Open **Settings → Voice → TTS** and tap **Test Voice**. If that fails, the issue is in the TTS pipeline; if it works, the issue is upstream in the conversation agent or HA's TTS service.

**Voice list is empty in the picker**
- The endpoint isn't reachable from the tablet. Confirm the TTS server's `/v1/audio/voices` URL responds from a device on the same network.
- For HA Piper, the picker pulls from the HA TTS service — check that HA's Piper add-on is running.

## See also

- [Setup Guide](../SETUP.md) — full onboarding walkthrough
- [MQTT Setup](mqtt.md) — trigger TTS from automations via the `dashvoice/<id>/command/tts` topic
