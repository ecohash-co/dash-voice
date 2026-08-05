# Privacy Policy

**Effective Date:** January 28, 2025
**Last Updated:** August 4, 2026

DashVoice ("we", "our", or "the app") is committed to protecting your privacy. This Privacy Policy explains how DashVoice handles information when you use our Android application.

---

## Summary

**DashVoice is designed with privacy as a core principle:**

- Wake word detection and speech recognition run **on the device**. Your audio never leaves the tablet — only the transcript travels, and only to the Home Assistant instance you configured
- We do **not** collect, store, or transmit your voice recordings to any third-party servers
- We do **not** sell or share your personal information
- DashVoice contains **no analytics SDK, no crash-reporting SDK and no third-party telemetry of any kind**
- Anything beyond the tablet is a server *you* chose. If you point Home Assistant at a cloud conversation agent, or configure a hosted text-to-speech endpoint, your transcripts and responses go wherever you sent them — that is your choice, not something DashVoice does on its own

---

## Information We Access

### Microphone Access
DashVoice requires microphone access to:
- Detect the wake word ("Hey Jarvis") using on-device processing
- Capture voice commands for speech recognition

**How it's processed:**
- Wake word detection runs entirely on your device using the openWakeWord library
- Speech recognition can run either:
  - **On-device** using sherpa-onnx (no network required)
  - **On your Home Assistant server** using your configured STT engine

**We never:**
- Record or store your voice
- Send audio to third-party cloud services
- Use your voice data for training or analytics

### Camera Access (Optional)
If enabled, DashVoice may use your camera for:
- QR code scanning during setup
- Optional motion detection, used to wake the screen

Motion detection compares successive camera frames on the device to see whether anything changed. **There is no face detection and no face recognition** — DashVoice does not identify people. Camera frames are processed locally, are not stored, and are never transmitted.

### Network Access
DashVoice connects to:
- **Your Home Assistant server** - to process commands and display dashboards
- **Your MQTT broker** (if configured) - for real-time status updates
- **Your Immich/ImmichFrame server** (if configured) - to display photos

All connections are made to servers **you configure** on your local network or your own cloud infrastructure.

---

## Information We Collect

**Nothing.**

DashVoice contains no analytics SDK, no crash-reporting SDK and no third-party telemetry of any kind. Nothing is collected, so there is nothing to opt out of.

### We Do NOT Collect
- Crash logs, stack traces or diagnostic reports
- Device or app identifiers of any kind
- Voice recordings or transcripts
- Home Assistant credentials or data
- Personal information
- Location data
- Usage analytics
- Advertising identifiers

---

## Data Storage

### On Your Device
DashVoice stores the following locally on your device:
- App settings and preferences
- Home Assistant connection details (encrypted)
- Downloaded speech recognition models
- Cached dashboard data

### On Your Servers
Your voice commands are processed by your Home Assistant instance. Please refer to your Home Assistant privacy settings for how that data is handled.

---

## Third-Party Services

DashVoice may connect to third-party services **that you configure**:

| Service | Purpose | Data Shared |
|---------|---------|-------------|
| Home Assistant | Command processing, dashboards | Voice command text |
| MQTT Broker | Real-time updates | Device status |
| TTS Endpoint | Text-to-speech | Response text |
| Immich | Photo display | None (read-only) |

These are services **you control**. We have no access to your data on these services.

There are no other third parties. DashVoice does not embed any analytics, crash-reporting, advertising or telemetry service, and sends nothing to us or to Google beyond what Google Play itself handles when you install or update the app.

---

## Children's Privacy

DashVoice is not directed at children under 13. We do not knowingly collect information from children.

---

## Data Security

- Home Assistant tokens are stored using Android's encrypted SharedPreferences
- All network connections use HTTPS/TLS when available
- No sensitive data leaves your local network (unless you configure external access)

---

## Your Rights

You can:
- **Delete all app data** by uninstalling the app or clearing app data in Android settings
- **Revoke permissions** (microphone, camera) in Android settings

There is no data held by us to request, correct or delete — we never receive any.

---

## Changes to This Policy

We may update this Privacy Policy occasionally. Changes will be posted to this page with an updated "Last Updated" date.

---

## Contact Us

If you have questions about this Privacy Policy:

- **GitHub Issues:** [github.com/ecohash-co/dash-voice/issues](https://github.com/ecohash-co/dash-voice/issues)
- **Email:** privacy@ecohash.co

---

## Open Source Components

DashVoice uses open-source libraries for on-device processing:
- [openWakeWord](https://github.com/dscripka/openWakeWord) - Wake word detection (Apache 2.0)
- [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) - Speech recognition (Apache 2.0)

These libraries process data entirely on your device.
