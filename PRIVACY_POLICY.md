# Privacy Policy

**Effective Date:** January 28, 2025
**Last Updated:** January 28, 2025

DashVoice ("we", "our", or "the app") is committed to protecting your privacy. This Privacy Policy explains how DashVoice handles information when you use our Android application.

---

## Summary

**DashVoice is designed with privacy as a core principle:**

- All voice processing happens **on your device** or **your own Home Assistant server**
- We do **not** collect, store, or transmit your voice recordings to any third-party servers
- We do **not** sell or share your personal information
- The app works entirely within your local network

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
- Future: Presence detection (processed on-device)

Camera data is processed locally and never transmitted.

### Network Access
DashVoice connects to:
- **Your Home Assistant server** - to process commands and display dashboards
- **Your MQTT broker** (if configured) - for real-time status updates
- **Your Immich/ImmichFrame server** (if configured) - to display photos

All connections are made to servers **you configure** on your local network or your own cloud infrastructure.

---

## Information We Collect

### Crash Reports (Optional)
If you opt in, DashVoice uses Firebase Crashlytics to collect:
- App crash logs and stack traces
- Device model and Android version
- App version

This helps us fix bugs and improve stability. Crash reports do **not** contain:
- Voice recordings
- Personal information
- Home Assistant data

You can opt out of crash reporting in the app settings.

### We Do NOT Collect
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

### Firebase (Google)
If crash reporting is enabled:
- Firebase Crashlytics receives crash logs
- Subject to [Google's Privacy Policy](https://policies.google.com/privacy)

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
- **Disable crash reporting** in app settings
- **Revoke permissions** (microphone, camera) in Android settings

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
