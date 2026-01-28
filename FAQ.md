# Frequently Asked Questions

## General

### What is DashVoice?
DashVoice turns any Android tablet into a smart home voice assistant and dashboard. It integrates with Home Assistant to let you control your smart home with voice commands like "Hey Jarvis, turn on the lights."

### Is DashVoice free?
DashVoice is a paid app available on Google Play. We believe in sustainable development and providing quality support.

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
Currently, DashVoice supports "Hey Jarvis". More wake words are planned for future updates.

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
When idle, DashVoice can show a photo slideshow from your Immich server using ImmichFrame. It's a beautiful way to display family photos when you're not using voice commands.

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

## Future Plans

### What features are coming?
We're working on:
- Additional wake word options
- Multi-language support
- Presence detection (face detection to wake screen)
- Intercom features between tablets
- And more based on your feedback!

### How can I request a feature?
Open a feature request: [Request a Feature](https://github.com/ecohash-co/dash-voice/issues/new?template=feature_request.md)

We read every request and prioritize based on community interest.
