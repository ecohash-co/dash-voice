# DashVoice

**Transform any Android tablet into a smart home voice assistant, dashboard, and multiroom speaker.**

DashVoice is a privacy-focused voice assistant for Android tablets that integrates with [Home Assistant](https://www.home-assistant.io/) and [Music Assistant](https://music-assistant.io/). Turn an old tablet into a wall-mounted smart home controller with always-on wake word detection, voice commands, a customizable dashboard, and synchronized multiroom audio.

[![Google Play](https://img.shields.io/badge/Google_Play-Early_Access-green?style=for-the-badge&logo=google-play)](https://play.google.com/store/apps/details?id=com.dashvoice)
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

We wanted to say "Hey Jarvis, turn on the lights" and have it just work. No cloud services, no monthly fees, no privacy concerns. Just a tablet on the wall that listens for a wake word and controls our smart home.

Then we wanted music. Not just on one tablet, but synchronized across every room. So we added [SendSpin](https://github.com/music-assistant/aiosendspin) multiroom audio, and now every DashVoice tablet is also a speaker in your whole-home audio system.

So we built DashVoice.

---

## Features

### Voice Control
- **Custom wake words** - "Hey Jarvis", "Okay Nabu", and more (fully on-device via [openWakeWord](https://github.com/dscripka/openWakeWord))
- **On-device speech recognition** - Using [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx), or leverage the [Wyoming protocol](https://www.home-assistant.io/integrations/wyoming/) for [Home Assistant's voice pipeline](https://www.home-assistant.io/voice_control/)
- **Natural language commands** - Via [Home Assistant Conversation API](https://www.home-assistant.io/integrations/conversation/)
- **High-quality TTS** - Via [Piper](https://github.com/rhasspy/piper) or OpenAI-compatible endpoints like [Kokoro](https://github.com/remsky/Kokoro-FastAPI)
- **Announcements** - "Hey Jarvis, announce dinner is ready" broadcasts to every tablet in the house: full-screen card, chime, and spoken message *(new)*
- **Native timers** - Multiple named timers, fully offline, with full-screen alarms - "set a pizza timer for 12 minutes" *(new)*
- **Voice music control** - "Play LCD Soundsystem radio on the main floor": on-device music intents drive Music Assistant search, radio mode, and synchronized multi-room zones - no LLM needed ([guide](docs/VOICE_MUSIC.md), *coming in the next release*)

### Multiroom Audio (SendSpin)
- **[Music Assistant](https://music-assistant.io/) integration** - Your tablets become speakers in your whole-home audio system
- **Synchronized playback** - Sub-50ms time-synced audio across all DashVoice tablets using the [SendSpin protocol](https://github.com/music-assistant/aiosendspin)
- **Now Playing overlay** - Glassmorphic UI with album art, playback controls, and track info
- **System volume control** - Music Assistant controls your tablet's actual volume; group members get relative gain
- **Multi-codec support** - FLAC, PCM, and Opus decoding via Android MediaCodec
- **Automatic discovery** - Tablets register via mDNS and appear in Music Assistant automatically
- **Voice-controlled zones** - Group any rooms into named zones ("main floor", "everywhere") and start synchronized playback by voice ([Voice Music guide](docs/VOICE_MUSIC.md))

### Smart Dashboard
- **[Home Assistant](https://www.home-assistant.io/) WebView** - Display any Lovelace dashboard with full SPA routing support
- **Screensaver mode** - Show photos from [Immich](https://immich.app/) via [ImmichFrame](https://github.com/3rob3/ImmichFrame), or use any URL (DAKboard, weather displays, custom pages)
- **Night mode** - Dim red clock display (iOS StandBy-style, preserves night vision)
- **Auto-brightness** - Logarithmic brightness curve with configurable dim/wake thresholds and ambient light hysteresis
- **Slide-out control drawer** - Quick access to brightness, volume, mic mute, screensaver, and settings

### Home Assistant Integration
- **MQTT auto-discovery** - Device sensors and controls appear automatically in Home Assistant
- **Wyoming satellite** - Native HA voice pipeline satellite support
- **Device sensors** - Battery level, ambient light, charging state, and motion exposed to HA
- **Remote configuration** - Control dashboard URL, screensaver, wake word, TTS settings, and more via MQTT or HTTP API
- **[Fully Kiosk](https://www.fully-kiosk.com/) compatible API** - Works with the HA Fully Kiosk integration out of the box

### Privacy First
- Wake word detection runs **100% on-device**
- Speech recognition can run **entirely locally** (no cloud required)
- No audio is ever sent to third parties
- Secrets stored in Android's encrypted SharedPreferences (AES-256, Keystore-backed)
- **No analytics SDK, no crash-reporting SDK, no third-party telemetry of any kind** — nothing is collected, so there is nothing to opt out of
- All processing happens on your tablet and your [Home Assistant](https://www.home-assistant.io/) instance

### Tablet-Friendly
- Works great on budget tablets (Samsung Galaxy Tab S7 FE, Lenovo Tab M10, etc.)
- Optimized for wall-mounted kiosk use
- Supports landscape and portrait orientations
- Low power consumption in standby
- OTA updates via HTTP API for headless deployments

### Easy Multi-Tablet Setup
- **Guided onboarding** - A setup wizard walks you through Home Assistant connection, wake word, voice, and a Power Features step that auto-detects MQTT on your network
- **Copy settings between tablets** - Set up your first tablet, then new tablets discover it over the network and import its configuration in one tap — no re-entering URLs, agents, or endpoints

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

## Requirements

- Android tablet (Android 8.0+, arm64)
- [Home Assistant](https://www.home-assistant.io/) instance on your network
- Wi-Fi network
- Optional: [MQTT broker](https://www.home-assistant.io/integrations/mqtt/) for real-time device control and automations
- Optional: [Music Assistant](https://music-assistant.io/) for multiroom audio

---

## Quick Start

1. **Install DashVoice** from Google Play
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

The Google Play build targets 64-bit ARM (Play's 16 KB page-size requirement). If your tablet is **older or 32-bit** — common with LineageOS on legacy hardware — and the Play build won't install, grab the **legacy sideload APK** instead:

**[→ Latest legacy sideload release](https://github.com/ecohash-co/dash-voice/releases)**

It includes 32-bit ARM (`armeabi-v7a`) support and runs on **Android 8.0+**. It's best-effort and unsupported — these are older, low-RAM devices, so voice runs but will be slower, and DashVoice automatically selects its lightest on-device engine to fit. Sideload it by enabling "Install unknown apps" for your browser or file manager.

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
