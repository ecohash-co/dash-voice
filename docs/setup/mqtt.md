# MQTT Setup

DashVoice publishes its state and accepts commands over MQTT, using Home Assistant's MQTT Discovery so every entity appears automatically. Once it's wired up you can put DashVoice tablets on dashboards, drive them from automations, and see at a glance whether each one is online — without touching any HA YAML.

This guide assumes you're running Home Assistant. Setup takes one of three paths depending on how you run HA.

## What you get

After enabling MQTT, every DashVoice tablet exposes these entities to Home Assistant automatically:

**Sensors**
- `sensor.dashvoice_<name>_battery` — battery %
- `sensor.dashvoice_<name>_lux` — ambient light (lx)
- `sensor.dashvoice_<name>_wifi_signal` — Wi-Fi signal (dBm)
- `sensor.dashvoice_<name>_screen_brightness` — current brightness (0–255)
- `sensor.dashvoice_<name>_conversation_state` — voice assistant state (idle/listening/processing/speaking)

**Binary sensors**
- `binary_sensor.dashvoice_<name>_charging` — charger plugged in
- `binary_sensor.dashvoice_<name>_screen_on` — screen state
- `binary_sensor.dashvoice_<name>_screensaver_active` — screensaver running

**Controls**
- `switch.dashvoice_<name>_screen` — turn the screen on/off
- `number.dashvoice_<name>_brightness` — brightness slider
- `switch.dashvoice_<name>_screensaver` — start/stop screensaver
- `button.dashvoice_<name>_reload` — reload the current dashboard
- `text.dashvoice_<name>_tts` — type or template-in text to speak
- `text.dashvoice_<name>_url` — navigate to a URL

The tablet also publishes an availability topic, so Home Assistant marks entities unavailable when the tablet goes offline.

## Step 1 — Install an MQTT broker

### If you run Home Assistant OS or Supervised

Use the **Mosquitto broker** add-on. It's the official path and the easiest to set up.

1. **Settings → Add-ons → Add-on Store**
2. Find **Mosquitto broker** under Official add-ons → install
3. **Start** the add-on, and check **"Start on boot"** and **"Watchdog"**
4. **Configuration** tab — leave defaults. The add-on auto-creates an internal user.
5. Go to **Settings → People → Users → Add user** and create a dedicated MQTT user (e.g. `dashvoice`). Make sure **"Can only log in from the local network"** is off, since the tablet doesn't authenticate from `localhost`.

### If you run HA in Docker or HA Core

Run Mosquitto yourself:

```yaml
# docker-compose.yml
services:
  mosquitto:
    image: eclipse-mosquitto:2
    container_name: mosquitto
    restart: unless-stopped
    ports:
      - "1883:1883"
    volumes:
      - ./mosquitto/config:/mosquitto/config
      - ./mosquitto/data:/mosquitto/data
      - ./mosquitto/log:/mosquitto/log
```

Minimal `mosquitto/config/mosquitto.conf`:

```
listener 1883
persistence true
persistence_location /mosquitto/data/
allow_anonymous false
password_file /mosquitto/config/passwd
```

Create the password file:

```bash
docker run --rm -v $PWD/mosquitto/config:/mosquitto/config \
  eclipse-mosquitto:2 mosquitto_passwd -c -b /mosquitto/config/passwd dashvoice YOUR_PASSWORD
docker compose up -d mosquitto
```

## Step 2 — Add the MQTT integration in Home Assistant

1. **Settings → Devices & Services → Add Integration → MQTT**
2. **Broker:** `core-mosquitto` if you used the add-on, otherwise the host running Mosquitto
3. **Port:** `1883`
4. **Username / Password:** the MQTT user you created
5. Submit. Home Assistant will test the connection.

Auto-discovery is on by default; nothing else to configure here.

## Step 3 — Configure DashVoice

Recent versions of DashVoice auto-detect MQTT during onboarding. On the **Power Features** step, the app probes your Home Assistant instance and, if it finds the MQTT integration, shows a green **"Detected"** badge and pre-fills the broker URL. Just enter the MQTT user and password and you're done.

To configure an already-onboarded device, open **Settings → Power Features → MQTT** and set:

| Field | Value |
|---|---|
| Broker URL | `tcp://<ha-host>:1883` |
| Username | `dashvoice` (whatever you created) |
| Password | the MQTT password |
| Enabled | on |
| HA Discovery | on |

You can also do it from the command line via the Fully Kiosk-compatible HTTP API (replace `<tablet-ip>` and `<api-password>`):

```bash
TABLET=<tablet-ip>
PASSWORD=<api-password>

curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=mqttBrokerUrl&value=tcp://homeassistant.local:1883"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=mqttUsername&value=dashvoice"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=mqttPassword&value=YOUR_MQTT_PASSWORD"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setBooleanSetting&key=mqttEnabled&value=true"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setBooleanSetting&key=mqttDiscoveryEnabled&value=true"
```

Within a few seconds you should see the device appear under **Settings → Devices & Services → MQTT**.

## Topic reference

State topics (DashVoice publishes):
```
dashvoice/<device_id>/battery
dashvoice/<device_id>/lux
dashvoice/<device_id>/wifi_signal
dashvoice/<device_id>/screen_brightness
dashvoice/<device_id>/conversation_state
dashvoice/<device_id>/charging
dashvoice/<device_id>/screen_on
dashvoice/<device_id>/screensaver_active
dashvoice/<device_id>/availability
```

Command topics (DashVoice subscribes):
```
dashvoice/<device_id>/command/screen        # "on" or "off"
dashvoice/<device_id>/command/brightness    # 0-255
dashvoice/<device_id>/command/screensaver   # "start" or "stop"
dashvoice/<device_id>/command/reload        # any payload
dashvoice/<device_id>/command/tts           # text to speak
dashvoice/<device_id>/command/url           # URL to navigate to
```

`<device_id>` is a stable per-install UUID. You can read it from the tablet's `deviceInfo` HTTP response or the MQTT discovery payload.

## Real screen off (optional)

`screen on/off` toggles the Android display only if you've granted DashVoice **Device Admin** permission. Without it, "off" starts the screensaver instead.

To grant it: open Settings on the tablet → **Security → Device admin apps** → enable **DashVoice**.

## Troubleshooting

**No entities appear in Home Assistant**

```bash
# Subscribe to discovery topics and confirm DashVoice is publishing
mosquitto_sub -h <broker> -u <user> -P <pass> -t "homeassistant/+/dashvoice_+/+/config" -v

# Subscribe to state to confirm liveness
mosquitto_sub -h <broker> -u <user> -P <pass> -t "dashvoice/#" -v
```

If both are silent, check that **MQTT** and **HA Discovery** toggles are both on in DashVoice settings, and verify the broker URL is reachable from the tablet.

**Entities appear but commands don't take effect**

```bash
# Confirm DashVoice is subscribed
mosquitto_sub -h <broker> -u <user> -P <pass> -t "dashvoice/+/command/#" -v
```

Send a test command and watch for the topic to fire. If nothing arrives, double-check the device ID — discovery uses the same ID as the command topics.

**Entities go unavailable after a few minutes**

DashVoice publishes a heartbeat every 30 s. If you're seeing flapping, the tablet is likely sleeping aggressively — disable battery optimization for DashVoice in **Settings → Battery → DashVoice → Don't optimize**.

## See also

- [Setup Guide](../SETUP.md) — full onboarding walkthrough
- [TTS Setup](tts.md) — control TTS from automations via the `dashvoice/<id>/command/tts` topic
