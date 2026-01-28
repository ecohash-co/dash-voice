# Awesome Smart Home

A curated list of open-source projects that work great with DashVoice.

---

## Core Platform

### Home Assistant
The heart of your smart home. Open-source home automation that puts local control and privacy first.

- **Website:** [home-assistant.io](https://www.home-assistant.io/)
- **GitHub:** [home-assistant/core](https://github.com/home-assistant/core)
- **Why we love it:** Privacy-focused, massive integration library, active community

---

## Voice & Conversation

### Extended OpenAI Conversation
Supercharge Home Assistant's conversation abilities with LLM-powered understanding.

- **GitHub:** [jekalmin/extended_openai_conversation](https://github.com/jekalmin/extended_openai_conversation)
- **Why we love it:** Natural language understanding, can control multiple devices with one command, works with local LLMs

### Piper
Fast, local neural text-to-speech. No cloud required.

- **GitHub:** [rhasspy/piper](https://github.com/rhasspy/piper)
- **Home Assistant:** [Piper Integration](https://www.home-assistant.io/integrations/piper/)
- **Why we love it:** Runs locally, high quality voices, fast response times

### Whisper
OpenAI's speech recognition, running locally on your hardware.

- **GitHub:** [openai/whisper](https://github.com/openai/whisper)
- **Home Assistant:** [Whisper Integration](https://www.home-assistant.io/integrations/whisper/)
- **Why we love it:** Excellent accuracy, multi-language support, runs locally

### Kokoro TTS
High-quality neural TTS with expressive voices.

- **GitHub:** [remsky/Kokoro-FastAPI](https://github.com/remsky/Kokoro-FastAPI)
- **Why we love it:** Studio-quality voices, OpenAI-compatible API, runs on modest hardware

### openWakeWord
On-device wake word detection. Powers DashVoice's "Hey Jarvis" detection.

- **GitHub:** [dscripka/openWakeWord](https://github.com/dscripka/openWakeWord)
- **Why we love it:** 100% on-device, customizable wake words, low resource usage

### sherpa-onnx
On-device speech recognition. Powers DashVoice's local ASR.

- **GitHub:** [k2-fsa/sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx)
- **Why we love it:** Fast, accurate, runs on mobile devices, multiple languages

---

## Photo Display

### Immich
Self-hosted photo and video backup solution. Like Google Photos, but you own your data.

- **Website:** [immich.app](https://immich.app/)
- **GitHub:** [immich-app/immich](https://github.com/immich-app/immich)
- **Why we love it:** Beautiful UI, face recognition, mobile apps, active development

### ImmichFrame
Turn any screen into a digital photo frame showing your Immich photos.

- **GitHub:** [3rob3/ImmichFrame](https://github.com/3rob3/ImmichFrame)
- **Why we love it:** Perfect screensaver for DashVoice, shows your memories, easy setup

---

## Hardware & Integration

### ESPHome
Create custom smart home devices with simple YAML configuration.

- **Website:** [esphome.io](https://esphome.io/)
- **GitHub:** [esphome/esphome](https://github.com/esphome/esphome)
- **Why we love it:** DIY sensors and controllers, native HA integration, no cloud

### Zigbee2MQTT
Use Zigbee devices without proprietary hubs.

- **Website:** [zigbee2mqtt.io](https://www.zigbee2mqtt.io/)
- **GitHub:** [Koenkk/zigbee2mqtt](https://github.com/Koenkk/zigbee2mqtt)
- **Why we love it:** Supports 2000+ devices, local control, no cloud dependency

### Z-Wave JS
Modern Z-Wave integration for Home Assistant.

- **GitHub:** [zwave-js/zwave-js](https://github.com/zwave-js/zwave-js)
- **Why we love it:** Reliable, well-maintained, great HA integration

---

## Monitoring & Media

### Frigate
AI-powered NVR for Home Assistant. Real-time object detection for your cameras.

- **Website:** [frigate.video](https://frigate.video/)
- **GitHub:** [blakeblackshear/frigate](https://github.com/blakeblackshear/frigate)
- **Why we love it:** Local AI detection, integrates with HA, low latency streams

### go2rtc
Ultimate camera streaming tool. WebRTC, RTSP, and more.

- **GitHub:** [AlexxIT/go2rtc](https://github.com/AlexxIT/go2rtc)
- **Why we love it:** Low-latency streaming, works with any camera, Frigate companion

---

## Local AI

### Ollama
Run large language models locally.

- **Website:** [ollama.ai](https://ollama.ai/)
- **GitHub:** [ollama/ollama](https://github.com/ollama/ollama)
- **Why we love it:** Easy setup, works with Extended OpenAI Conversation, privacy

### LocalAI
Self-hosted, OpenAI-compatible API for running LLMs locally.

- **GitHub:** [mudler/LocalAI](https://github.com/mudler/LocalAI)
- **Why we love it:** Drop-in replacement for OpenAI API, multiple model support

---

## Getting Started

New to self-hosted smart home? Here's a recommended learning path:

1. **Start with Home Assistant** - Install on a Raspberry Pi or mini PC
2. **Add some devices** - Start with Zigbee bulbs or smart plugs
3. **Set up Piper** - Get local TTS working
4. **Install Extended OpenAI Conversation** - Better voice understanding
5. **Add DashVoice** - Wall-mount a tablet for voice control
6. **Set up Immich + ImmichFrame** - Beautiful photo screensaver

---

## Contributing

Know a great project that should be on this list? [Open an issue](https://github.com/ecohash-co/dash-voice/issues) or submit a pull request!

---

*This list is curated by the DashVoice team. We're not affiliated with any of these projects - we just think they're awesome!*
