# DashVoice

**Transform any Android tablet into a smart home voice assistant and dashboard.**

DashVoice is a privacy-focused voice assistant for Android tablets that integrates with [Home Assistant](https://www.home-assistant.io/). Turn an old tablet into a wall-mounted smart home controller with always-on wake word detection, voice commands, and a customizable dashboard.

[![Google Play](https://img.shields.io/badge/Google_Play-Coming_Soon-green?style=for-the-badge&logo=google-play)](https://play.google.com/store/apps/details?id=com.dashvoice)

---

## Why DashVoice?

DashVoice was born out of frustration. For years, we used [Fully Kiosk Browser](https://www.fully-kiosk.com/) with [Home Assistant](https://www.home-assistant.io/) to create wall-mounted tablet dashboards. Fully Kiosk is excellent for displaying dashboards, but it couldn't do the one thing we really wanted: **voice control**.

We wanted to say "Hey Jarvis, turn on the lights" and have it just work. No cloud services, no monthly fees, no privacy concerns. Just a tablet on the wall that listens for a wake word and controls our smart home.

So we built DashVoice.

---

## Features

### Voice Control
- **Custom wake word support** - "Hey Jarvis", "Okay Nabu", and more (fully on-device via [openWakeWord](https://github.com/dscripka/openWakeWord))
- **On-device speech recognition** - Using [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx), or leverage the [Wyoming protocol](https://www.home-assistant.io/integrations/wyoming/) for [Home Assistant's voice pipeline](https://www.home-assistant.io/voice_control/)
- **Natural language commands** - Via [Home Assistant Conversation API](https://www.home-assistant.io/integrations/conversation/)
- **High-quality TTS** - Via [Piper](https://github.com/rhasspy/piper) or OpenAI-compatible endpoints like [Kokoro](https://github.com/remsky/Kokoro-FastAPI)

### Smart Dashboard
- **[Home Assistant](https://www.home-assistant.io/) WebView** - Display any Lovelace dashboard
- **Screensaver mode** - Show photos from [Immich](https://immich.app/) via [ImmichFrame](https://github.com/3rob3/ImmichFrame), or use any URL (DAKboard, weather displays, custom pages)
- **Night mode** - Dim red clock display (preserves night vision)
- **Auto-brightness** - Adjusts based on ambient light sensor

### Privacy First
- Wake word detection runs **100% on-device**
- Speech recognition can run **entirely locally** (no cloud required)
- No audio is ever sent to third parties
- All processing happens on your tablet and your [Home Assistant](https://www.home-assistant.io/) instance

### Tablet-Friendly
- Works great on budget tablets (Lenovo Tab M10, etc.)
- Optimized for wall-mounted kiosk use
- Supports landscape and portrait orientations
- Low power consumption in standby

---

## Screenshots

*Coming soon*

---

## Requirements

- Android tablet (Android 8.0+, arm64)
- [Home Assistant](https://www.home-assistant.io/) instance
- Wi-Fi network
- Optional: [MQTT broker](https://www.home-assistant.io/integrations/mqtt/) for enhanced status updates and device control

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

---

## Support

- **Bug reports**: [Open an issue](https://github.com/ecohash-co/dash-voice/issues/new?template=bug_report.md)
- **Feature requests**: [Open an issue](https://github.com/ecohash-co/dash-voice/issues/new?template=feature_request.md)
- **Questions**: [GitHub Discussions](https://github.com/ecohash-co/dash-voice/discussions)

---

## Related Projects

DashVoice works great with these open-source projects:

### Core Platform
| Project | Description |
|---------|-------------|
| [Home Assistant](https://www.home-assistant.io/) | Open-source home automation platform |
| [MQTT Integration](https://www.home-assistant.io/integrations/mqtt/) | Message broker for real-time device updates |
| [Wyoming Protocol](https://www.home-assistant.io/integrations/wyoming/) | Home Assistant's local voice assistant pipeline |

### Voice & AI
| Project | Description |
|---------|-------------|
| [Extended OpenAI Conversation](https://github.com/jekalmin/extended_openai_conversation) | LLM-powered conversation for Home Assistant |
| [openWakeWord](https://github.com/dscripka/openWakeWord) | On-device wake word detection |
| [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) | On-device speech recognition |
| [Piper](https://github.com/rhasspy/piper) | Fast, local neural text-to-speech |
| [Whisper](https://www.home-assistant.io/integrations/whisper/) | OpenAI's speech recognition (runs locally) |

### Display & Media
| Project | Description |
|---------|-------------|
| [Immich](https://immich.app/) | Self-hosted photo management |
| [ImmichFrame](https://github.com/3rob3/ImmichFrame) | Digital photo frame for Immich |
| [Fully Kiosk Browser](https://www.fully-kiosk.com/) | The app that inspired DashVoice's dashboard features |

---

## License

DashVoice is proprietary software. This repository contains documentation and issue tracking only.

---

*Made with care for the smart home community*
