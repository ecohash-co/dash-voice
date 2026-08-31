# Frequently Asked Questions

## General

### What is DashVoice?
DashVoice turns any Android tablet into a smart home voice assistant and dashboard. It integrates with Home Assistant to let you control your smart home with voice commands like "Hey Jarvis, turn on the lights."

### How much does DashVoice cost?
DashVoice is **$9.99 on Google Play** — a one-time purchase, not a subscription.

A Play purchase is tied to your Google account rather than to a device, so **one purchase covers every tablet in your house.** That matters here: multiroom audio and house-wide announcements only do anything once you have a second tablet, and we didn't want the price to argue against the feature.

### Then why is there a free APK on GitHub?
Because some tablets can't install the Play build. Google Play requires 64-bit ARM, which rules out older and 32-bit devices — exactly the hardware this app is meant to give a second life to. The [legacy sideload APK](https://github.com/ecohash-co/dash-voice/releases) exists for those tablets and carries an optional tip link instead, since a sideloaded app can't charge through Play.

It isn't a free tier. If your tablet can install from Play, that's the supported build: it updates automatically and it's the one we test against.

### What tablets work with DashVoice?
Any Android tablet running Android 8.0+ with an arm64 processor. We've tested extensively on:
- Lenovo Tab M10 (great budget option)
- Samsung Galaxy Tab S7+
- Most tablets from the last 5 years

### Does it work on phones?
Technically yes, but DashVoice is optimized for tablets used as wall-mounted or tabletop smart home controllers.

---

## Privacy & Security

### Does DashVoice listen to everything I say?
No. DashVoice only processes audio after detecting the wake word ("Hey Jarvis"). The wake word detection runs entirely on your device - no audio is sent anywhere until you activate it.

### Where does my voice data go?
Your voice is processed either:
1. **On-device** (default) - Using sherpa-onnx, everything stays on your tablet
2. **Your Home Assistant** - If you choose HA-based STT, audio goes to YOUR server

We never send audio to third-party cloud services.

### Is DashVoice "100% local and private"?
**Wake word and speech recognition run on the tablet.** Microphone audio is not uploaded to Ecohash or to a DashVoice cloud — there isn't one.

That is not the same as "nothing you say ever touches a network." After the transcript exists, DashVoice sends it to **your** Home Assistant. If that conversation agent is a cloud LLM, the text goes there because *you* configured it. TTS and Music Assistant work the same way. Timers stay on the device. Announcements stay on your MQTT (and HA if you use it).

Nothing in the 0.1.587 release made this worse. Sendspin audio on your LAN is now **encrypted** (it used to be a plaintext WebSocket). Optional camera JPEG and Loki logging only happen if you turn them on, and they go to servers you name.

### Is my Home Assistant token secure?
Yes. Your token is stored using Android's encrypted storage and only used to communicate with your Home Assistant instance.

---

## Setup

### How do I get a Home Assistant Long-Lived Access Token?
1. Open Home Assistant
2. Click your profile (bottom left)
3. Scroll to "Long-Lived Access Tokens"
4. Click "Create Token"
5. Give it a name like "DashVoice Kitchen"
6. Copy the token (you won't see it again!)

### Do I need a separate token for each tablet?
Yes, we recommend creating a separate token for each DashVoice tablet. This lets you revoke access to individual devices if needed.

### What's the difference between Local ASR and Home Assistant ASR?
- **Local ASR**: Speech recognition runs on your tablet. Faster, works offline, more private.
- **Home Assistant ASR**: Uses your HA's speech-to-text service (Whisper, etc.). May be more accurate for some languages.

### Why can't DashVoice find my Home Assistant?
Common issues:
- Make sure your tablet is on the same network as Home Assistant
- Try using the IP address instead of hostname (e.g., `192.168.1.100:8123`)
- Check that Home Assistant is accessible from a browser on the tablet
- Ensure your firewall allows connections on port 8123

---

## Voice Commands

### What wake words are available?
DashVoice supports "Hey Jarvis", "Okay Nabu", and other wake words via [openWakeWord](https://github.com/dscripka/openWakeWord). You can also import custom wake word models.

### Why isn't the wake word detected?
- Make sure microphone permission is granted
- Check that the tablet's microphone isn't blocked
- Speak clearly from within a few feet of the tablet
- Adjust the wake word sensitivity in settings

### What commands can I use?
Any command your Home Assistant understands! Examples:
- "Hey Jarvis, turn on the kitchen lights"
- "Hey Jarvis, what's the temperature inside?"
- "Hey Jarvis, lock the front door"
- "Hey Jarvis, play music in the living room"

The actual capabilities depend on your Home Assistant setup and conversation agent.

Some commands are handled directly on the tablet, without Home Assistant's conversation agent:
- "Hey Jarvis, set a 10 minute timer" — [native timers](#announcements--timers)
- "Hey Jarvis, announce dinner is ready" — [announcements](#announcements--timers) on every tablet
- "Hey Jarvis, play jazz in the kitchen" — [voice music control](docs/VOICE_MUSIC.md)

### How do I improve voice recognition accuracy?
- Use Local ASR with the NeMo Fast model (default) for best latency
- Speak clearly and at a normal pace
- Reduce background noise if possible
- Make sure you're within 6-10 feet of the tablet

---

## Dashboard & Display

### Can I show my own Home Assistant dashboard?
Yes! In settings, you can specify any Lovelace dashboard URL. The dashboard displays in a full-screen WebView.

### What is the screensaver?
When idle, DashVoice can show a photo slideshow from your Immich server using ImmichFrame (or any URL). Photos stay up during a voice command. The strip can include Love, Wand, Fewer-like-this, Next, and Exit when you use rustFrame.

### What's the dim red clock?
At night, DashVoice can show a dim red clock instead of the bright dashboard. Red light doesn't disrupt your sleep or night vision. Tap anywhere to wake it up.

### How do I adjust brightness?
DashVoice can automatically adjust brightness based on ambient light (if your tablet has a light sensor). You can also set a fixed brightness in settings.

---

## Troubleshooting

### The app crashes when I try to use voice commands
- Make sure you've granted microphone permission
- Try restarting the app
- Check that the ASR model has downloaded completely
- If issues persist, please [report a bug](https://github.com/ecohash-co/dash-voice/issues)

### Voice commands aren't being understood correctly
- Check your Home Assistant conversation agent settings
- Try using the Extended OpenAI Conversation integration for better understanding
- Ensure your HA entities have friendly names

### The dashboard won't load
- Verify your Home Assistant URL is correct
- Check that your access token is valid
- Try accessing the dashboard URL in a browser first
- Make sure your HA instance is accessible from the tablet's network

### How do I report a bug?
Open an issue on GitHub: [Report a Bug](https://github.com/ecohash-co/dash-voice/issues/new?template=bug_report.md)

Please include:
- Your tablet model and Android version
- Steps to reproduce the issue
- Any error messages you see

---

## Multiroom Audio

### How does multiroom audio work?
DashVoice uses the [Sendspin protocol](https://github.com/Sendspin/aiosendspin) to receive synchronized audio streams from [Music Assistant](https://music-assistant.io/). Each tablet registers itself via mDNS and appears as a speaker in Music Assistant, where you can group tablets and control playback. From **0.1.587** that link is **encrypted** (Noise). In Music Assistant, open the player → **Setup** → allow playback **without pairing**. Leave **Allow legacy clients** on until that player has played a track.

### Why did Music Assistant create a second "Kitchen DashVoice"?
The encrypted identity is a new key, not the old MAC-looking id. Delete or ignore the old player once the 43-character id is playing. Don't turn off Allow legacy clients until then.

### Do I need Music Assistant for multiroom audio?
Yes. Music Assistant is a free, open-source music player for Home Assistant that handles music sources (Spotify, local files, etc.) and sends synchronized audio to DashVoice tablets via Sendspin.

### Can I control music with voice commands?
Yes — two ways:

1. **Native voice music control** - DashVoice matches music commands on-device and drives Music Assistant directly: "Hey Jarvis, play LCD Soundsystem radio on the main floor" searches, groups the zone's speakers, and starts synchronized playback — no LLM or conversation agent needed. See the [Voice Music guide](docs/VOICE_MUSIC.md).
2. **Via Home Assistant** - Any music command your HA conversation agent understands works as before.

### Can I play music in multiple rooms with one command?
Yes, with native voice music control. Every Music Assistant player name is a zone automatically, "everywhere" targets all speakers, and you can define custom zones like "main floor" in the `musicZones` setting. Details in the [Voice Music guide](docs/VOICE_MUSIC.md).

### What audio formats are supported?
DashVoice supports FLAC (lossless), PCM (uncompressed), and Opus (compressed) audio streams. Music Assistant selects the best format automatically.

---

## Announcements & Timers

### How do announcements work?
Say "Hey Jarvis, announce dinner is ready" (or "broadcast...", "tell everyone...") and every DashVoice tablet in the house shows a full-screen announcement card, plays a chime, and speaks your message. It's handled entirely on-device — no conversation agent involved.

### What do announcements require?
Your tablets need to share an MQTT broker — that's how they talk to each other. See the [MQTT Setup guide](docs/setup/mqtt.md). Everything else is automatic.

### Does DashVoice have timers?
Yes — native, fully offline timers. Say "set a 10 minute timer", "set a pizza timer for 12 minutes", or even "set a timer for an hour and a half". You can run multiple named timers at once, and pause, resume, cancel, or ask how much time is left by voice. When a timer fires, you get a full-screen alarm — dismiss it with a tap or by saying "stop".

### Do timers need Home Assistant?
No. Timers run entirely on the tablet and survive app restarts, so they work even if your network or Home Assistant is down.

---

## Future Plans

### What features are coming?
Still on the list: follow-up questions without the wake word, voice intercom (your recorded voice between tablets), and AirPlay. TTS voice pickers, announcements, native timers, and a Home Assistant integration are already in the current release.

### How can I request a feature?
Open a feature request: [Request a Feature](https://github.com/ecohash-co/dash-voice/issues/new?template=feature_request.md)

We read every request and prioritize based on community interest.
