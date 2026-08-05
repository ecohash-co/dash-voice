# DashVoice - Play Store Listing

> This file is the source of truth for what should be in the Play Console. If you
> change the listing, change it here too — otherwise the repo and the Console drift.

## App Name
**DashVoice — Voice, Dashboard & Multiroom for Home Assistant**

If the length is rejected: `DashVoice — Voice & Dashboard for Home Assistant`

---

## Short Description (80 characters max)
```
Local wake word, HA dashboard, and whole-home audio on any Android tablet
```
(73 characters)

---

## Full Description (4000 characters max)

```
Give the tablet in your drawer a second life.

DashVoice turns an Android tablet into three things at once: a voice assistant that
listens on the device, a Home Assistant dashboard, and a speaker in your whole-home
audio system. Say "Hey Jarvis, turn on the kitchen lights" and it just works — the wake
word and the speech recognition run on the tablet itself.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

VOICE THAT RUNS ON THE TABLET

• Wake word detection entirely on-device — "Hey Jarvis", "Okay Nabu" and others
• On-device speech recognition, no internet round-trip to understand you
• Your audio never leaves the tablet. Only the transcript goes to your own Home
  Assistant, and only after the wake word
• Commands are handled by whichever conversation agent you've set up in HA
• Natural, high-quality speech responses via Piper or any OpenAI-compatible endpoint

ANSWERS WITHOUT THE CLOUD

• Timers run on the device — "set a pizza timer for twelve minutes" — with named
  timers, a full-screen alarm, and no dependency on your network being up
• Announcements broadcast to every tablet in the house: "Hey Jarvis, announce that
  dinner is ready" lights up each screen with a card, a chime and the spoken message
• Both keep working when Home Assistant is unreachable

WHOLE-HOME AUDIO

• Each tablet registers as a speaker in Music Assistant
• Synchronized playback across tablets, measured within about 50 ms
• Ask for music by voice — "play LCD Soundsystem radio on the main floor" — including
  named zones you define, without needing an LLM agent
• Now Playing overlay with album art and transport controls; group volume handled
  properly

A DASHBOARD WORTH MOUNTING

• Any Home Assistant Lovelace dashboard, full-screen
• Photo screensaver — point it at Immich/ImmichFrame or any URL
• Night mode with a dim red clock that preserves night vision
• Auto-brightness from the ambient light sensor, with hysteresis so it doesn't flicker
• Slide-out drawer for brightness, volume, mic mute and settings

FITS INTO HOME ASSISTANT PROPERLY

• MQTT auto-discovery — sensors and controls appear in HA on their own
• Battery, ambient light, charging state and motion exposed as entities
• Remote configuration over MQTT or HTTP
• Compatible with the Fully Kiosk integration's API
• Wyoming satellite support for HA's own voice pipeline

SET UP THE SECOND TABLET IN ONE TAP

• A guided wizard handles Home Assistant, wake word, voice and power features
• A new tablet finds an existing one over your network and imports its configuration

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WHAT YOU NEED

• An Android tablet, 8.0 or newer
• A Home Assistant instance on your network
• Wi-Fi

DashVoice has no account, no subscription and no analytics or crash-reporting SDK.
Music Assistant is optional and only needed for audio features.

Documentation and FAQ: github.com/ecohash-co/dash-voice
Issues: github.com/ecohash-co/dash-voice/issues
```

---

## Category
**House & Home** (primary)

---

## Tags/Keywords

Work these in naturally:

- home assistant
- music assistant
- multiroom
- wake word
- tablet dashboard
- kiosk
- local voice
- immich

---

## Data Safety

Declares **no data collected**. Firebase was removed from the app on 2026-05-25 and
DashVoice contains no analytics SDK, no crash-reporting SDK and no third-party
telemetry of any kind. The privacy policy says the same thing — keep them in agreement.

---

## Content Rating
- No violence
- No sexual content
- No profanity
- No drugs/alcohol
- Suitable for all ages

**Likely rating: Everyone**

---

## Permissions Explanation (for Play Console)

| Permission | Reason |
|------------|--------|
| Microphone | Required for wake word detection and voice commands. Wake word and speech recognition run on the device |
| Camera | Optional — QR code scanning during setup, and optional motion detection (frame differencing on the device, used to wake the screen). **No face detection and no face recognition** |
| Internet | Connect to your Home Assistant instance, and to any TTS or Music Assistant endpoint you configure |
| Foreground Service | Keep voice detection running when the screen is off |

---

## Privacy Policy URL
```
https://github.com/ecohash-co/dash-voice/blob/main/PRIVACY_POLICY.md
```

---

## Feature Graphic Suggestions (1024x500)

Ideas for the feature graphic:
1. Tablet mounted on wall showing dashboard + voice waveform
2. "Hey Jarvis" text with microphone icon + Home Assistant logo
3. Split view: tablet dashboard on left, privacy shield icon on right
4. Clean dark background with DashVoice logo + tagline
