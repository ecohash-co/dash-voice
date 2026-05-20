# Loki Setup

DashVoice can ship every log line — wake-word detections, ASR transcripts, network errors, crashes — to a [Loki](https://grafana.com/oss/loki/) server. Once that's wired up you can query all of your tablets from Grafana and spot regressions before you notice them in daily use.

If you only run one DashVoice tablet, this is optional. If you have three or more, it pays for itself the first time something goes wrong.

## What you get

Every log line is tagged with:
- `job=dashvoice`
- `device_name` — the name you set during onboarding
- `tag` — the source component (`WakeWordService`, `LocalAsrHandler`, `HaConversationApi`, etc.)
- `level` — `info`, `warn`, `error`

Which makes queries like these easy:

```logql
# Everything from one tablet
{job="dashvoice", device_name="kitchen"}

# Wake word firings across every tablet, last hour
{job="dashvoice"} |~ "Wake word detected"

# ASR final transcripts
{job="dashvoice", tag="LocalAsrHandler"} |~ "ASR final"

# Errors only
{job="dashvoice", level="error"}

# HA Conversation API failures
{job="dashvoice", tag="HaConversationApi"} |~ "error|fail|timeout"
```

> **The job label is `dashvoice` — with an underscore.** Older builds used a different label; if you're upgrading and your queries return nothing, this is why.

## Step 1 — Run Loki

Pick whichever fits your setup. If you don't already have a logs stack, the docker-compose path is the fastest.

### Option A — Loki + Grafana via docker-compose

```yaml
# docker-compose.yml
services:
  loki:
    image: grafana/loki:3.0.0
    container_name: loki
    restart: unless-stopped
    ports:
      - "3100:3100"
    volumes:
      - ./loki/data:/loki
      - ./loki/config.yaml:/etc/loki/local-config.yaml:ro
    command: -config.file=/etc/loki/local-config.yaml

  grafana:
    image: grafana/grafana:11.0.0
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - GF_AUTH_ANONYMOUS_ENABLED=true
      - GF_AUTH_ANONYMOUS_ORG_ROLE=Admin
    volumes:
      - ./grafana/data:/var/lib/grafana
    depends_on:
      - loki
```

Minimal `loki/config.yaml`:

```yaml
auth_enabled: false

server:
  http_listen_port: 3100

common:
  path_prefix: /loki
  storage:
    filesystem:
      chunks_directory: /loki/chunks
      rules_directory: /loki/rules
  replication_factor: 1
  ring:
    instance_addr: 127.0.0.1
    kvstore:
      store: inmemory

schema_config:
  configs:
    - from: 2024-01-01
      store: tsdb
      object_store: filesystem
      schema: v13
      index:
        prefix: index_
        period: 24h

ingester:
  wal:
    enabled: true
    dir: /loki/wal

limits_config:
  retention_period: 168h        # 7 days; raise once you trust it
  max_query_length: 168h
```

Bring it up:

```bash
docker compose up -d loki grafana
```

In Grafana (`http://<host>:3000`):
1. **Connections → Data sources → Add data source → Loki**
2. URL: `http://loki:3100`
3. Save & test

### Option B — Loki via a Home Assistant add-on

If you already use Grafana on Home Assistant, the community add-on store has a Loki add-on. Install it, configure HTTP on port 3100, and add it as a data source in your existing Grafana instance.

### Option C — Grafana Cloud (hosted)

Grafana Cloud has a free tier with 50 GB of log ingestion. Sign up, then in DashVoice settings use the Loki **push URL** from your stack (`https://logs-prod-XXX.grafana.net/loki/api/v1/push`) and a Cloud API key. DashVoice supports basic auth in the URL: `https://user:apikey@logs-prod-XXX.grafana.net`.

## Step 2 — Configure DashVoice

On the onboarding wizard's **Power Features** step, or anytime via **Settings → Power Features → Loki Logging**:

| Field | Value |
|---|---|
| Loki URL | `http://<loki-host>:3100/loki/api/v1/push` |
| Enabled | on |

From the HTTP API (replace `<tablet-ip>` and `<api-password>`):

```bash
TABLET=<tablet-ip>
PASSWORD=<api-password>

curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setStringSetting&key=lokiUrl&value=http://loki.local:3100/loki/api/v1/push"
curl "http://$TABLET:2324/?password=$PASSWORD&cmd=setBooleanSetting&key=lokiLoggingEnabled&value=true"
```

Within ~10 seconds you should see lines arrive. Verify with:

```bash
curl -s "http://loki.local:3100/loki/api/v1/labels" | jq
# Should list "device_name", "job", "tag", "level"
```

## Step 3 — Dashboard

Grafana's built-in Logs panel is good enough to start. A more useful starter dashboard:

- **Panel 1** — `count_over_time({job="dashvoice"} |~ "Wake word detected" [1h])` by `device_name`. Wake-word activity per tablet.
- **Panel 2** — `count_over_time({job="dashvoice", level="error"} [1h])` by `device_name`. Error rate across all tablets.
- **Panel 3** — Live log stream with filters for `device_name` and `tag`.

## Troubleshooting

**No logs in Loki, no errors on the tablet**

```bash
# From the tablet's network, confirm the push endpoint is reachable
curl -X POST "http://loki.local:3100/loki/api/v1/push" \
  -H "Content-Type: application/json" \
  -d '{"streams":[{"stream":{"job":"test"},"values":[["'$(date +%s%N)'","hello"]]}]}'
```

If that returns 204, the network is fine — check the URL in DashVoice settings (the push path is `/loki/api/v1/push`, not `/loki`).

**Logs stop after Wi-Fi drops**

DashVoice buffers a small number of lines in memory. If Wi-Fi is out for more than a few minutes, you'll lose older lines. This is acceptable for most setups; if you need durable buffering, run a [promtail](https://grafana.com/docs/loki/latest/clients/promtail/) instance and ship from there instead.

## See also

- [MQTT Setup](mqtt.md) — companion observability surface (device state, not logs)
- [Loki LogQL cheat sheet](https://grafana.com/docs/loki/latest/logql/) — query language reference
