# DashVoice

**Turn any Android tablet into a private smart display.**

DashVoice is a smart home voice assistant, a [Home Assistant](https://www.home-assistant.io/) dashboard, and a synchronized multiroom speaker in one app. Wake word and speech recognition run entirely on the tablet — your voice never leaves your network.

Give the tablet in your drawer a second life: mount it on the wall, say *"Hey Jarvis, turn on the kitchen lights"*, and it just works. The tablet isn't only showing your smart home — it's part of it. It listens, it answers, it announces, it rings, and it plays music in sync with every other tablet in the house.

[![Google Play](https://img.shields.io/badge/Google_Play-Available-green?style=for-the-badge&logo=google-play)](https://play.google.com/store/apps/details?id=com.dashvoice)
[![Android](https://img.shields.io/badge/Android-8.0%2B-blue?style=for-the-badge&logo=android)](https://developer.android.com/)

<p align="center">
  <a href="https://youtu.be/f7RMHgUMX_c">
    <img src="https://img.youtube.com/vi/f7RMHgUMX_c/maxresdefault.jpg" alt="DashVoice Demo" width="600">
  </a>
  <br>
  <strong>▶️ Watch the demo</strong>
</p>

---

## Why DashVoice?

DashVoice was born out of frustration. For years, we used [Fully Kiosk Browser](https://www.fully-kiosk.com/) with [Home Assistant](https://www.home-assistant.io/) to create wall-mounted tablet dashboards. Fully Kiosk is excellent for displaying dashboards, but it couldn't do the one thing we really wanted: **voice control**.

We wanted to say "Hey Jarvis, turn on the lights" and have it just work — without shipping the audio of our house to somebody's server, and without a monthly fee. Just a tablet on the wall that listens for a wake word and controls our smart home.

Then we wanted music. Not just on one tablet, but synchronized across every room. So we added [SendSpin](https://github.com/music-assistant/aiosendspin) multiroom audio, and now every DashVoice tablet is also a speaker in your whole-home audio system.

So we built DashVoice.

---

## Features

### Voice, fully on-device
- **Custom wake words** - "Hey Jarvis", "Okay Nabu", and more (fully on-device via [openWakeWord](https://github.com/dscripka/openWakeWord))
- **On-device speech recognition** - Using [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx), so there's no internet round-trip just to understand you. Or leverage the [Wyoming protocol](https://www.home-assistant.io/integrations/wyoming/) for [Home Assistant's voice pipeline](https://www.home-assistant.io/voice_control/)
- **Your audio never leaves the tablet** - Only the transcript travels, only after the wake word, and only to your own Home Assistant
- **Natural language commands** - Handled by whichever conversation agent you've set up in HA, via the [Conversation API](https://www.home-assistant.io/integrations/conversation/)
- **High-quality TTS** - Via [Piper](https://github.com/rhasspy/piper) or OpenAI-compatible endpoints like [Kokoro](https://github.com/remsky/Kokoro-FastAPI)
- **Typically 1-2 seconds** from wake word to spoken reply on Tab S7-class hardware

### Answers without the cloud
Some things shouldn't need a server at all. Timers and announcements are matched and handled on the tablet itself — they keep working when Home Assistant is unreachable.

- **Native timers** - *"set a pizza timer for twelve minutes"*. Multiple named timers at once, pause/resume/cancel by voice, a full-screen alarm when they fire, and they survive an app restart
- **Announcements** - *"announce that dinner is ready"* lights up every tablet in the house with a full-screen card, a chime, and the spoken message
- **No conversation agent involved** - no LLM, no network round-trip, no cloud dependency

See [Announcements & timers](#announcements--timers) below for the phrases that work.

### Whole-home audio (multiroom)
- **[Music Assistant](https://music-assistant.io/) integration** - Each tablet registers as a speaker in your whole-home audio system
- **Synchronized playback** - Time-synced audio across DashVoice tablets using the [SendSpin protocol](https://github.com/music-assistant/aiosendspin), **measured within about 50 ms** across tablets
- **Voice music control** - *"play LCD Soundsystem radio on the main floor"*: on-device music intents drive Music Assistant search, radio mode, and synchronized multi-room zones — no LLM needed ([guide](docs/VOICE_MUSIC.md))
- **Voice-controlled zones** - Group any rooms into named zones ("main floor", "everywhere") and start synchronized playback by voice
- **Now Playing overlay** - Glassmorphic UI with album art, playback controls, and track info
- **Group transport from any tablet** - Stop, pause or skip on one tablet and the whole group follows
- **System volume control** - Music Assistant controls your tablet's actual volume; group members get relative gain
- **Multi-codec support** - FLAC, PCM, and Opus decoding via Android MediaCodec
- **Automatic discovery** - Tablets register via mDNS and appear in Music Assistant automatically

### A dashboard worth mounting
- **[Home Assistant](https://www.home-assistant.io/) WebView** - Display any Lovelace dashboard with full SPA routing support
- **Screensaver mode** - Show photos from [Immich](https://immich.app/) via [ImmichFrame](https://github.com/3rob3/ImmichFrame), or use any URL (DAKboard, weather displays, custom pages)
- **Night mode** - Dim red clock display (iOS StandBy-style, preserves night vision)
- **Auto-brightness** - Logarithmic brightness curve with configurable dim/wake thresholds and ambient light hysteresis
- **Slide-out control drawer** - Quick access to brightness, volume, mic mute, screensaver, and settings
- **Photo feedback strip** - Heart or skip a screensaver photo without leaving the screensaver

### Fits into Home Assistant properly
- **MQTT auto-discovery** - Device sensors and controls appear automatically in Home Assistant
- **Wyoming satellite** - Native HA voice pipeline satellite support
- **Device sensors** - Battery level, ambient light, charging state, and motion exposed to HA
- **Remote configuration** - Control dashboard URL, screensaver, wake word, TTS settings, and more via MQTT or HTTP API
- **[Fully Kiosk](https://www.fully-kiosk.com/) compatible API** - Works with the HA Fully Kiosk integration out of the box

### Set up the second tablet in one tap
- **Guided onboarding** - A setup wizard walks you through Home Assistant connection, wake word, voice, and a Power Features step that auto-detects MQTT on your network
- **Copy settings between tablets** - Set up your first tablet, then new tablets discover it over the network and import its configuration in one tap — no re-entering URLs, agents, or endpoints

### Privacy
The claim worth making is the specific one: **wake word detection and speech recognition run on the device.** Your audio never leaves the tablet. What travels is the transcript, only after the wake word, and only to the Home Assistant instance you configured.

- **No analytics SDK, no crash-reporting SDK, no third-party telemetry of any kind** — nothing is collected, so there is nothing to opt out of
- **No account, no subscription** — DashVoice doesn't have a login and there is nothing to sign up for
- Secrets stored in Android's encrypted SharedPreferences (AES-256, Keystore-backed)
- Timers and announcements never leave the tablet at all

What DashVoice deliberately doesn't claim: that nothing you say ever touches a cloud. That part is your choice. Your HA conversation agent may be a cloud LLM, your TTS endpoint may be hosted, and Music Assistant providers are usually streaming services. All three are configured by you, and all three can be local if you want them to be. See the [Privacy Policy](PRIVACY_POLICY.md).

### Compatibility
- Android 8.0 or newer; arm64 on Google Play, with a [legacy sideload build](#which-build-do-i-want) for 32-bit ARM
- Works great on budget tablets (Samsung Galaxy Tab S7 FE, Lenovo Tab M10, etc.)
- The small streaming speech model runs on essentially any 64-bit device; the 0.6B high-accuracy models need roughly 7 GB of RAM and won't load below that
- Optimized for wall-mounted kiosk use, landscape and portrait
- Low power consumption in standby
- OTA updates via HTTP API for headless deployments

---

## Announcements & timers

The two features that demo instantly and cost nothing to try. Both run entirely on the tablet — no conversation agent, no LLM, and no dependency on Home Assistant being up.

**Announcements** broadcast to every DashVoice tablet in the house: a full-screen card, a chime, and the spoken message.

- *"Hey Jarvis, announce that dinner is ready"*
- *"Hey Jarvis, broadcast the movie is starting"*
- *"Hey Jarvis, tell everyone it's time to leave"*

Announcements need your tablets to share an MQTT broker — that's how they find each other. See the [MQTT setup guide](docs/setup/mqtt.md).

**Timers** are native, named, and fully offline. Run several at once, ask how long is left, and get a full-screen alarm when one fires — dismiss it with a tap or by saying "stop".

- *"Hey Jarvis, set a 10 minute timer"*
- *"Hey Jarvis, set a pizza timer for twelve minutes"*
- *"Hey Jarvis, set a timer for an hour and a half"*
- *"Hey Jarvis, how much time is left on the pasta timer?"*

And a few more things handled on the tablet, with everything else falling through to your Home Assistant agent untouched:

| Say this | What happens |
|---|---|
| *"play LCD Soundsystem radio on the main floor"* | Artist radio, synced across the zone |
| *"play the dinner party playlist in the kitchen"* | Playlists, albums and tracks by name |
| *"turn it down"* / *"next song"* / *"stop the music"* | Zone-aware transport and volume |
| *"turn on the kitchen lights"* | Passed straight to Home Assistant |

---

## Screenshots

### Voice Control in Action
| Listening | Processing |
|-----------|------------|
| ![Listening](assets/screenshots/21-voice-response.jpg) | ![Processing](assets/screenshots/20-voice-listening.jpg) |

### Onboarding
| Welcome | Connect to HA | Wake Word Selection |
|---------|---------------|---------------------|
| ![Welcome](assets/screenshots/01-welcome.png) | ![Connection](assets/screenshots/04-connection.png) | ![Wake Word](assets/screenshots/08-wake-word.png) |

### Voice Configuration
| Voice Processing | TTS & Agent | Hotword Import |
|------------------|-------------|----------------|
| ![Voice Processing](assets/screenshots/07-voice-processing.png) | ![Voice Agent](assets/screenshots/11-voice-agent.png) | ![Hotwords](assets/screenshots/12-hotwords.png) |

### Settings
| Home Assistant | Presence Detection | MQTT |
|----------------|-------------------|------|
| ![Settings HA](assets/screenshots/17-settings-full.png) | ![Presence](assets/screenshots/18-settings-presence.png) | ![MQTT](assets/screenshots/19-settings-mqtt.png) |

<details>
<summary>View all onboarding screenshots</summary>

| Step | Screenshot |
|------|------------|
| Welcome | ![](assets/screenshots/01-welcome.png) |
| Permissions | ![](assets/screenshots/02-permissions.png) |
| Device Discovery | ![](assets/screenshots/03-discovery.png) |
| HA Connection | ![](assets/screenshots/04-connection.png) |
| Authentication | ![](assets/screenshots/05-authentication-instructions.png) |
| Auth Success | ![](assets/screenshots/05-authentication-success.png) |
| Device Name | ![](assets/screenshots/06-device-name.png) |
| Voice Processing | ![](assets/screenshots/07-voice-processing.png) |
| Wake Word | ![](assets/screenshots/08-wake-word.png) |
| Voice Enrollment | ![](assets/screenshots/09-voice-enrollment.png) |
| Voice Output | ![](assets/screenshots/10-voice-output.png) |
| Conversation Agent | ![](assets/screenshots/11-voice-agent.png) |
| Hotword Import | ![](assets/screenshots/12-hotwords.png) |
| Dashboard Setup | ![](assets/screenshots/13-dashboard-screensaver.png) |
| Complete | ![](assets/screenshots/14-complete.png) |

</details>

---

## Requirements & what's optional

**Required**

- An Android tablet running Android 8.0 or newer
- A [Home Assistant](https://www.home-assistant.io/) instance on your network — HA is the brain; DashVoice is the face, the ears and the speaker
- Wi-Fi

**Optional**

| | what it adds | without it |
|---|---|---|
| [Music Assistant](https://music-assistant.io/) | Multiroom audio and voice music control | Everything else works; the tablet just isn't a speaker |
| [MQTT broker](https://www.home-assistant.io/integrations/mqtt/) | Device sensors and controls in HA, plus house-wide announcements | Voice, dashboard and timers are unaffected |
| [Immich](https://immich.app/) / [ImmichFrame](https://github.com/3rob3/ImmichFrame) | Photo screensaver | Point the screensaver at any URL instead, or turn it off |

**Optional, and entirely your call: the cloud.** Wake word and speech recognition are always on the device. Beyond that, the conversation agent that answers you is whichever one you configured in Home Assistant — that may be a local one or a cloud LLM. Text-to-speech may be local [Piper](https://github.com/rhasspy/piper) or a hosted OpenAI-compatible endpoint. Music Assistant providers are usually streaming services. DashVoice doesn't pick any of those for you, and a fully local setup is a supported configuration.

---

## Which build do I want?

| | **Google Play** (recommended) | **Legacy sideload APK** |
|---|---|---|
| For | Anything from roughly the last several years | Older or 32-bit tablets, LineageOS on legacy hardware |
| Price | $9.99 one-time — a Play purchase covers **every tablet on your Google account** | Free; optional tip link, since a sideloaded app can't charge through Play |
| Architecture | arm64 (Play's 16 KB page-size requirement) | Adds 32-bit ARM (`armeabi-v7a`) |
| Android | 8.0+ | 8.0+ |
| Updates | Automatic | Manual — check back here |
| Support | Supported | Best-effort, unsupported |
| Get it | [Play Store](https://play.google.com/store/apps/details?id=com.dashvoice) | [v0.1.553-legacy](https://github.com/ecohash-co/dash-voice/releases/tag/v0.1.553-legacy) |

**The sideload build is not a free tier.** Play requires 64-bit ARM, which rules out
exactly the older hardware this app exists to revive — so that build is there for tablets
Play can't reach, and it runs on the honour system.

Install from Google Play unless it refuses to install on your tablet. If it does refuse — usually a 32-bit device — take the legacy build. These are older, low-RAM devices, so voice works but runs slower. On first run DashVoice sizes up your tablet's RAM and pre-selects a speech engine that fits, and falls back to the lighter pipeline if the modern one can't start; it won't override an engine you pick yourself, so if you change it, choose a small one. To sideload, enable "Install unknown apps" for your browser or file manager, then open the downloaded APK.

---

## Quick Start

1. **Install DashVoice** from [Google Play](https://play.google.com/store/apps/details?id=com.dashvoice) (or the [legacy APK](#which-build-do-i-want) for older devices)
2. **Complete onboarding** - connect to your [Home Assistant](https://www.home-assistant.io/)
3. **Generate a Long-Lived Access Token** in Home Assistant ([instructions](https://www.home-assistant.io/docs/authentication/#your-account-profile))
4. **Say "Hey Jarvis"** and start controlling your smart home!

For detailed setup instructions, see the [Setup Guide](docs/SETUP.md).

---

## Documentation

- [Setup Guide](docs/SETUP.md) - Complete installation walkthrough
- [FAQ](FAQ.md) - Frequently asked questions
- [Privacy Policy](PRIVACY_POLICY.md) - How we handle your data
- [Awesome Smart Home](docs/AWESOME.md) - Related projects we recommend

### Power Feature Guides

These optional integrations add a lot of value once you're up and running:

- [MQTT Setup](docs/setup/mqtt.md) - Auto-discover every tablet as device sensors and controls in Home Assistant
- [Loki Setup](docs/setup/loki.md) - Ship logs from all your tablets to Grafana for fleet-wide visibility
- [Text-to-Speech Setup](docs/setup/tts.md) - Piper vs. Kokoro vs. OpenAI — quality, latency, and cost compared
- [Voice Music Control](docs/VOICE_MUSIC.md) - Play music in any room (or every room) by voice via Music Assistant — zones, radio mode, and multi-room sync

---

## Support

- **Bug reports**: [Open an issue](https://github.com/ecohash-co/dash-voice/issues/new?template=bug_report.md)
- **Feature requests**: [Open an issue](https://github.com/ecohash-co/dash-voice/issues/new?template=feature_request.md)
- **Questions**: [GitHub Discussions](https://github.com/ecohash-co/dash-voice/discussions)

### ☕ Support development

DashVoice is built and maintained by one developer. If it's useful to you — especially if you're running it on hardware the Play Store can't reach — a tip helps keep it going:

**[buymeacoffee.com/ecohash_co](https://buymeacoffee.com/ecohash_co)**

---

## Older or 32-bit devices (LineageOS, Android 8.x)

See [Which build do I want?](#which-build-do-i-want) above — the legacy sideload APK adds 32-bit ARM (`armeabi-v7a`) support for tablets the Play build can't reach.

**[→ Latest legacy sideload release](https://github.com/ecohash-co/dash-voice/releases/tag/v0.1.553-legacy)**

---

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                          DashVoice                           │
│                                                              │
│  ┌─────────────────┐      ┌─────────────────┐               │
│  │  openWakeWord   │      │   sherpa-onnx   │               │
│  │  "Hey Jarvis"   │─────▶│   Local ASR     │               │
│  └─────────────────┘      └────────┬────────┘               │
│                                    │                         │
│                                    ▼                         │
│                           ┌─────────────────┐               │
│                           │ HA Conversation │               │
│                           │      API        │               │
│                           └────────┬────────┘               │
│                                    │                         │
│                                    ▼                         │
│                           ┌─────────────────┐               │
│                           │  TTS Playback   │               │
│                           │ (Kokoro/Piper)  │               │
│                           └─────────────────┘               │
│                                                              │
│  ┌─────────────────┐      ┌─────────────────┐               │
│  │    SendSpin     │      │   Now Playing   │               │
│  │  Audio Client   │─────▶│    Overlay      │               │
│  └─────────────────┘      └─────────────────┘               │
│                                                              │
│  ┌─────────────────┐      ┌─────────────────┐               │
│  │    Wyoming      │      │     MQTT        │               │
│  │   Satellite     │      │   Heartbeat     │               │
│  └─────────────────┘      └─────────────────┘               │
└──────────────────────────────────────────────────────────────┘
```

---

## Related Projects

DashVoice works great with these open-source projects:

### Core Platform
| Project | Description |
|---------|-------------|
| [Home Assistant](https://www.home-assistant.io/) | Open-source home automation platform |
| [Music Assistant](https://music-assistant.io/) | Universal music player for Home Assistant (multiroom audio) |
| [MQTT Integration](https://www.home-assistant.io/integrations/mqtt/) | Message broker for real-time device updates |
| [Wyoming Protocol](https://www.home-assistant.io/integrations/wyoming/) | Home Assistant's local voice assistant pipeline |

### Voice & AI
| Project | Description |
|---------|-------------|
| [Extended OpenAI Conversation](https://github.com/jekalmin/extended_openai_conversation) | LLM-powered conversation for Home Assistant |
| [openWakeWord](https://github.com/dscripka/openWakeWord) | On-device wake word detection |
| [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) | On-device speech recognition |
| [Piper](https://github.com/rhasspy/piper) | Fast, local neural text-to-speech |
| [Kokoro](https://github.com/remsky/Kokoro-FastAPI) | High-quality OpenAI-compatible TTS |
| [Whisper](https://www.home-assistant.io/integrations/whisper/) | OpenAI's speech recognition (runs locally) |

### Display & Media
| Project | Description |
|---------|-------------|
| [Immich](https://immich.app/) | Self-hosted photo management |
| [ImmichFrame](https://github.com/3rob3/ImmichFrame) | Digital photo frame for Immich |
| [SendSpin / aiosendspin](https://github.com/music-assistant/aiosendspin) | Multiroom audio sync protocol |
| [Fully Kiosk Browser](https://www.fully-kiosk.com/) | The app that inspired DashVoice's dashboard features |

---

## License

DashVoice is proprietary software. This repository contains documentation and issue tracking only.

---

*Made with care for the smart home community*
