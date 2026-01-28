# DashVoice

**Transform any Android tablet into a smart home voice assistant and dashboard.**

DashVoice is a privacy-focused voice assistant for Android tablets that integrates with Home Assistant. Turn an old tablet into a wall-mounted smart home controller with always-on wake word detection, voice commands, and a customizable dashboard.

[![Google Play](https://img.shields.io/badge/Google_Play-Coming_Soon-green?style=for-the-badge&logo=google-play)](https://play.google.com/store/apps/details?id=com.dashvoice)

---

## Features

### Voice Control
- **"Hey Jarvis"** wake word detection (fully on-device, no cloud)
- **On-device speech recognition** using sherpa-onnx (or Home Assistant STT)
- **Natural language commands** via Home Assistant Conversation API
- **High-quality TTS** via Piper or OpenAI-compatible endpoints (Kokoro)

### Smart Dashboard
- **Home Assistant WebView** - Display any Lovelace dashboard
- **Screensaver mode** - Show photos from Immich via ImmichFrame
- **Night mode** - Dim red clock display (preserves night vision)
- **Auto-brightness** - Adjusts based on ambient light

### Privacy First
- Wake word detection runs **100% on-device**
- Speech recognition can run **entirely locally** (no cloud required)
- No audio is ever sent to third parties
- All processing happens on your tablet and Home Assistant

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
- Home Assistant instance
- Wi-Fi network
- Optional: MQTT broker for enhanced features

---

## Quick Start

1. **Install DashVoice** from Google Play
2. **Complete onboarding** - connect to your Home Assistant
3. **Generate a Long-Lived Access Token** in Home Assistant
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

| Project | Description |
|---------|-------------|
| [Home Assistant](https://www.home-assistant.io/) | Open-source home automation platform |
| [Extended OpenAI Conversation](https://github.com/jekalmin/extended_openai_conversation) | Enhanced AI conversation for Home Assistant |
| [Immich](https://immich.app/) | Self-hosted photo management |
| [ImmichFrame](https://github.com/3rob3/ImmichFrame) | Digital photo frame for Immich |
| [Piper](https://github.com/rhasspy/piper) | Fast, local neural text-to-speech |
| [openWakeWord](https://github.com/dscripka/openWakeWord) | On-device wake word detection |
| [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) | On-device speech recognition |

---

## License

DashVoice is proprietary software. This repository contains documentation and issue tracking only.

---

*Made with care for the smart home community*
