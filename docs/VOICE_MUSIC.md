# Voice Music Control

> **Shipped in 0.1.479.** Enable it from **Settings → Audio → Voice Music Control**.

Say it, hear it — in any room, or every room:

> "Hey Jarvis, play LCD Soundsystem radio on the main floor"

DashVoice matches music commands **entirely on-device** (no LLM, no conversation agent required), searches your [Music Assistant](https://music-assistant.io/) library, groups the right speakers into a synchronized zone, and starts playback. You get an instant spoken confirmation while the music spins up in the background.

What it can do:

- **Play by name** — artists, albums, tracks, playlists, genres, resolved by Music Assistant search
- **Radio mode** — "…radio" / "…station" starts an endless station seeded by the artist
- **Multi-room zones** — target a room, a floor, or the whole house; members play in sync
- **Shuffle** — "shuffle LCD Soundsystem in the office"
- **Transport** — stop, pause, resume, and skip by voice

---

## Requirements

- [ ] [Home Assistant](https://www.home-assistant.io/) connected in DashVoice
- [ ] [Music Assistant](https://music-assistant.io/) installed in Home Assistant, with at least one music provider (Spotify, Apple Music, local library, etc.)
- [ ] Your tablet(s) visible as Music Assistant players — DashVoice's built-in [Sendspin](https://github.com/Sendspin/aiosendspin) support does this automatically once multiroom audio is enabled (mDNS, no IP). From **0.1.587** the link is encrypted. In Music Assistant: player → **Setup** → allow playback without pairing. Leave **Allow legacy clients** on until that player has played. The first encrypted connect may show a **new** player id (43 characters).

Any Music Assistant player works as a target — DashVoice tablets, but also any other speakers Music Assistant manages.

---

## Enabling It

Voice music control is **off by default** while it's experimental:

1. Open **Settings → Audio**
2. Find the **Voice Music Control** section
3. Turn on **Voice music commands**

That's it for single-room use — every Music Assistant player's name is already a valid room to target. For floors and whole-house zones, see [Zones](#zones) below.

---

## What You Can Say

DashVoice deliberately claims only commands that are *unambiguously* about music — either the command names a room/zone, or it names a media type (album, playlist, radio…). Everything else passes through to Home Assistant untouched, so your existing voice setup keeps working exactly as before.

| You say | What happens |
|---|---|
| "play LCD Soundsystem radio on the main floor" | Artist radio station, synchronized across the main-floor zone |
| "play LCD Soundsystem in the kitchen" | Plays the artist on the kitchen speaker |
| "put on some jazz everywhere" | Plays jazz on every available speaker, in sync |
| "play the album Sound of Silver" | Plays the album on this tablet |
| "play the dinner party playlist in the living room" | Plays your playlist in the living room |
| "shuffle LCD Soundsystem in the office" | Shuffled artist playback in the office |
| "stop the music in the kitchen" | Stops kitchen playback |
| "pause the music" / "resume the music" | Pause/resume (when this tablet is playing) |
| "next song" / "previous song" | Skip within the current queue |

And what intentionally does **not** trigger it (these go to Home Assistant as usual):

| You say | Why it passes through |
|---|---|
| "play LCD Soundsystem" | No room and no media type — too ambiguous to claim |
| "play the news" | Not a music command |
| "play Stranger Things on the TV" | "the TV" isn't a music zone |
| "stop" / "pause" (bare) | Reserved — a bare "stop" dismisses a ringing timer, and Home Assistant owns the rest |

**Phrasing notes:**

- Play verbs: *play*, *put on*, *throw on*, *shuffle*, *start playing* (a leading "please" or "can you" is fine)
- Rooms attach with *in / on / to / through / over*: "…in the kitchen", "…on the main floor"
- A trailing *radio*, *station*, or *mix* turns on radio mode
- Media types you can name: *album*, *artist*, *band*, *song*, *track*, *playlist*, *station*, *radio*, *mix*, *genre*

---

## Zones

A **zone** is a named group of speakers. Three kinds exist:

1. **Automatic** — every Music Assistant player is its own zone under its friendly name ("Kitchen", "Office"). Nothing to configure.
2. **Built-in** — *everywhere*, *all speakers*, *every room*, *the whole house* target all available players at once.
3. **Custom** — your own multi-room zones ("main floor", "upstairs"), defined in the `musicZones` setting.

### Defining Custom Zones

Custom zones are configured as JSON in the `musicZones` setting:

```json
{
  "zones": [
    {
      "aliases": ["main floor", "downstairs"],
      "members": ["Kitchen", "Living Room"],
      "leader": "Kitchen"
    },
    {
      "aliases": ["upstairs"],
      "members": ["Bedroom", "Office"]
    }
  ]
}
```

- **aliases** — the phrases you'll say ("play jazz on the *main floor*"). Multiple aliases per zone are fine.
- **members** — Music Assistant player **friendly names**, exactly as they appear in Home Assistant. DashVoice resolves them to entities at command time, so they survive entity-ID changes.
- **leader** — optional; which member coordinates the group. Defaults to the first member.

When you target a multi-room zone, DashVoice groups the members through Music Assistant so they play in perfect sync — and ungroups stale members first, so a speaker borrowed by another group gets cleanly retargeted.

### Setting Zones via the HTTP API

There's no zones editor in the app yet — set the JSON through DashVoice's HTTP API (the same Fully Kiosk–compatible API used in the [MQTT](setup/mqtt.md) and [TTS](setup/tts.md) guides). Requires an API password set in **Settings → API**:

```bash
TABLET=<tablet-ip>
PASSWORD=<your-api-password>

curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=musicZones" \
  --data-urlencode 'value={"zones":[{"aliases":["main floor","downstairs"],"members":["Kitchen","Living Room"],"leader":"Kitchen"}]}' \
  -G
```

`musicZones` is a shared setting, so it's included when you copy settings between tablets — configure it once and the whole fleet understands the same zone names.

---

## How Playback Behaves

DashVoice confirms your command right away ("Playing LCD Soundsystem radio in the main floor"), then hands playback to Music Assistant in the background. Two things worth knowing:

- **Streaming providers rate-limit.** Spotify and Apple Music sometimes make Music Assistant wait before it can fetch tracks — so audio can occasionally take several seconds (rarely, longer) to start after the confirmation. That's the provider, not the tablet.
- **If search can't confirm your query quickly**, DashVoice says "Trying to play…" and lets Music Assistant resolve it in the background rather than making you wait.

---

## Troubleshooting

### Music commands go to Home Assistant instead of playing
- The feature is **off by default** — check Settings → Audio → Voice Music Control
- Bare "play \<artist\>" (no room, no media type) is *intentionally* passed to Home Assistant. Add a room ("…in the kitchen") or a media word ("play the album…")
- Zone names must match a Music Assistant player name or one of your `musicZones` aliases

### "I couldn't find \<query\>"
Music Assistant's search returned nothing. Check that the artist/album exists in one of your configured music providers, and try the name as Music Assistant lists it.

### "The \<room\> speaker is offline"
The target player is unavailable in Home Assistant. Check the tablet/speaker is powered, on the network, and shows as available in Music Assistant.

### "I can't reach Home Assistant"
DashVoice couldn't query Home Assistant in time. Verify the HA connection in Settings and network reachability.

### Music started but the confirmation said something slightly different
The spoken acknowledgment echoes your words; Music Assistant's search decides what actually plays. If it picked the wrong thing, try naming the media type: "play the **album** Sound of Silver".

### Two tablets both responded
If multiple tablets hear the same command, each may act on it — a known limitation of the experimental release. Mute the mic on tablets that don't need voice, or space out wake-word coverage.

---

## See Also

- [Setup Guide](SETUP.md) — getting DashVoice connected to Home Assistant
- [MQTT Setup](setup/mqtt.md) — the HTTP API password + remote configuration basics
- [FAQ](../FAQ.md) — multiroom audio questions
