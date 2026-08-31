# Changelog

## 0.1.587 (Google Play) / 0.1.588-legacy (GitHub sideload) — 2026-08-31

Play users get **0.1.587**. The GitHub APK is **0.1.588-legacy** (adds 32-bit ARM). Same app family as the previous 0.1.567 / 0.1.568-legacy pair.

### Highlights

- **Encrypted Music Assistant speakers.** Sendspin now speaks Noise (`KKpsk2`) with Music Assistant 2.10. The stream on your LAN is ciphertext, not a plaintext WebSocket. Enable Sendspin, let the tablet appear in Music Assistant, then on that player use **Setup → allow playback without pairing**. Leave **Allow legacy clients** on until that player has actually played. The first encrypted connect may create a **new** player id (43 characters, not a MAC-looking id).
- **Wake word after a quiet room.** Capture and speech decode no longer share one thread, so long silences drop fewer chunks. Automatic gain control is off on the wake mic.
- **Screensaver stays up while you talk.** Photos don’t vanish for a voice command. **Wand** is on the strip (Love · Wand · Fewer-like-this · Next · Exit) if you use rustFrame.
- **Optional camera JPEG** for Frigate / HA (`/cam.jpg`). **Off until you turn it on.**
- **Local HTTP API** fail-closes when the API password isn’t set (dangerous commands 403). Brute-force lockout on failed auth.
- **Android 16 / targetSdk 36.**

### Sendspin notes (if you already had a player)

Your tablet’s encrypted identity is a Curve25519 key, not the old MAC-format id. Music Assistant may keep playing the old player over the legacy hello until you remove that player and allow unpaired playback on the new one. Pairing PIN / CPace is **not** in this build — unpaired consent is the path.

### Not in this release

AirPlay, voice intercom, follow-up questions without the wake word.

---

## 0.1.567 (Google Play) / 0.1.568-legacy (GitHub) — 2026-08-05

Previous public cut. Voice music, announcements, native timers, and Sendspin playback were already in that line; this 0.1.587 cut hardens them and encrypts the speaker path.