# DashVoice Setup Guide

This guide walks you through setting up DashVoice on your Android tablet.

---

## Prerequisites

Before you begin, make sure you have:

- [ ] An Android tablet (Android 8.0+, arm64 processor)
- [ ] Home Assistant installed and accessible on your network
- [ ] Wi-Fi network that both tablet and Home Assistant can access
- [ ] About 500MB of free storage for the app and speech models

---

## Step 1: Install DashVoice

Download DashVoice from Google Play:

[![Google Play](https://img.shields.io/badge/Google_Play-Download-green?style=for-the-badge&logo=google-play)](https://play.google.com/store/apps/details?id=com.dashvoice)

---

## Step 2: Complete Onboarding

When you first launch DashVoice, you'll go through a setup wizard:

### 2.1 Welcome Screen
Tap **Get Started** to begin.

### 2.2 Permissions
Grant the required permissions:
- **Microphone** - Required for wake word detection and voice commands
- **Camera** (optional) - For QR code scanning during setup

### 2.3 Connect to Home Assistant

Enter your Home Assistant details:
- **Host**: Your HA URL (e.g., `homeassistant.local:8123` or `192.168.1.100:8123`)
- Tap **Test Connection** to verify

### 2.4 Authentication

You'll need a Long-Lived Access Token from Home Assistant:

1. Open Home Assistant in a browser
2. Click your profile icon (bottom left)
3. Scroll down to **Long-Lived Access Tokens**
4. Click **Create Token**
5. Name it something like "DashVoice Kitchen Tablet"
6. **Copy the token immediately** (you won't see it again!)
7. Paste it into DashVoice

> **Tip:** Create a separate token for each tablet. This lets you revoke access to individual devices if needed.

### 2.5 Device Name

Give your tablet a friendly name (e.g., "Kitchen", "Living Room"). This helps identify it in Home Assistant.

### 2.6 Voice Processing

Choose how voice commands are processed:

| Option | Pros | Cons |
|--------|------|------|
| **On-Device (Local)** | Faster, works offline, more private | Uses tablet storage (~500MB) |
| **Home Assistant** | Uses your existing HA STT setup | Requires network, depends on HA config |

We recommend **On-Device** for the best experience.

### 2.7 Download Speech Model

If you chose On-Device processing, the app will download the speech recognition model (~460MB). This only happens once.

### 2.8 Complete!

You're all set! Tap **Get Started** to begin using DashVoice.

---

## Step 3: Test Voice Commands

Try saying:

> "Hey Jarvis, what time is it?"

If everything is working, you'll hear a chime, see your command transcribed, and hear a response.

Try more commands:
- "Hey Jarvis, turn on the living room lights"
- "Hey Jarvis, what's the temperature outside?"
- "Hey Jarvis, is the front door locked?"

---

## Step 4: Configure Your Dashboard (Optional)

By default, DashVoice shows your Home Assistant's default Lovelace dashboard. To customize:

1. Open **Settings** (swipe from left edge or tap the gear icon)
2. Find **Dashboard URL**
3. Enter the URL of your preferred dashboard (e.g., `/dashboard-kitchen/0`)

---

## Step 5: Set Up Screensaver (Optional)

DashVoice can show photos from your Immich server when idle:

1. Set up [ImmichFrame](https://github.com/3rob3/ImmichFrame) on your network
2. In DashVoice Settings, enable **Screensaver**
3. Enter your ImmichFrame URL
4. Set the timeout (how long before screensaver activates)

---

## Recommended Home Assistant Setup

For the best experience, we recommend:

### Extended OpenAI Conversation

Install the [Extended OpenAI Conversation](https://github.com/jekalmin/extended_openai_conversation) integration for smarter voice command understanding. It can:
- Understand natural language better than the default
- Control multiple devices with one command
- Answer questions about your home

### Piper TTS

Install [Piper](https://www.home-assistant.io/integrations/piper/) for fast, high-quality text-to-speech. Voices we recommend:
- `en_US-ryan-medium` - Clear male voice
- `en_US-lessac-medium` - Natural female voice

### Alternative: Kokoro TTS

For even higher quality voices, set up [Kokoro](https://github.com/remsky/Kokoro-FastAPI) and use the OpenAI-compatible TTS option in DashVoice settings.

---

## Wall Mounting Tips

If you're mounting your tablet:

1. **Use a powered mount** - Keep the tablet plugged in 24/7
2. **Enable kiosk mode** - Prevents accidental app switching
3. **Disable screen timeout** - Let DashVoice manage the display
4. **Position for voice** - Mount at ear height for best wake word detection
5. **Consider ambient light** - Enable auto-brightness if your tablet supports it

---

## Troubleshooting

### Wake word not detected
- Ensure microphone permission is granted
- Check the microphone isn't covered
- Adjust sensitivity in Settings > Wake Word

### Commands not understood
- Verify Home Assistant connection in Settings
- Check your HA conversation agent is configured
- Try simpler commands first

### Dashboard won't load
- Test the URL in a browser first
- Verify your access token is valid
- Check network connectivity

For more help, see the [FAQ](../FAQ.md) or [open an issue](https://github.com/ecohash-co/dash-voice/issues).

---

## Next Steps

- Explore [Awesome Smart Home](AWESOME.md) for more integrations
- Join the community in [GitHub Discussions](https://github.com/ecohash-co/dash-voice/discussions)
- [Report bugs](https://github.com/ecohash-co/dash-voice/issues) or [request features](https://github.com/ecohash-co/dash-voice/issues)
