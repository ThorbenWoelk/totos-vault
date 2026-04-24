---
created: 2026-04-01
last_edited: 2026-04-24
tags:
- speaker-discovery
- local-network
- mdns
- bose
- soundtouch
- chromecast
- airplay
- procedure
connections: []
ai_generated: true
human_approved: false
category:
- AI Memory
- Procedures
---
Repeatable workflow for finding speaker devices from inside the local WLAN without preserving the real network map.

## When to use this

Use this when asked to:
- find speakers on the local WLAN
- identify cast or AirPlay devices
- check whether a Bose SoundTouch speaker is controllable
- determine the next likely API surface for a device

## Redaction rule

This note is a procedure, not an inventory. Keep live details out of it.

Do not commit:
- real local IPs or CIDRs
- friendly device names
- hostnames
- room names
- account IDs
- playlist names
- serial-like identifiers

Use placeholders such as `<local-cidr>`, `<speaker-host-or-ip>`, `<cast-host-or-ip>`, and `<airplay-host-or-ip>`.

## Discovery flow

### 1. Start with mDNS / Bonjour discovery

This is the highest-signal first step.

```bash
avahi-browse -art
```

For targeted checks, use service-specific discovery:

```bash
avahi-browse -rt _soundtouch._tcp
avahi-browse -rt _spotify-connect._tcp
avahi-browse -rt _googlecast._tcp
```

Useful service patterns:
- `_soundtouch._tcp` → Bose SoundTouch HTTP API, usually on port `8090`
- `_spotify-connect._tcp` → Spotify Connect-capable endpoint
- `_googlecast._tcp` → Chromecast / Google Cast device
- `AirTunes Remote Audio` → Apple audio target
- `AirPlay Remote Video` → Apple AirPlay video target

### 2. Run a subnet liveness scan for additional hosts

Derive the local CIDR from the runtime host.

```bash
ip -o -4 addr show scope global
```

Then scan the current LAN CIDR:

```bash
nmap -sn <local-cidr>
```

Use this to find hosts that are online but did not advertise enough information over mDNS.

### 3. Map each discovered service to a likely control path

### Bose SoundTouch

If `_soundtouch._tcp` is present:
- query `http://<speaker-host-or-ip>:8090/info`
- query `http://<speaker-host-or-ip>:8090/sources`
- query `http://<speaker-host-or-ip>:8090/presets`
- query `http://<speaker-host-or-ip>:8090/now_playing`
- query `http://<speaker-host-or-ip>:8090/volume`

Primary control path currently validated: `POST /select` with XML `ContentItem` payloads.

Use the generic SoundTouch checks in this note. Keep device-specific details in local runtime output.

### Chromecast / Google Cast

If `_googlecast._tcp` is present:
- treat it as a Cast target, not a simple REST speaker API
- mDNS gives identity, friendly name, and port information
- actual playback control usually requires Cast protocol tooling or an app-level sender
- keep friendly names in local runtime output only

### AirPlay targets

If AirPlay or AirTunes services appear:
- identify hostname and IP for the current session
- treat the device as a playback destination rather than assuming a simple local REST API
- AirPlay exposure is useful for discovery even if control requires a different client stack

## Bose SoundTouch operational recipe

Once a SoundTouch speaker is found:

1. Read `/info` to confirm the device
2. Read `/sources` to see what inputs are usable right now
3. Read `/presets` to harvest reusable `ContentItem` payloads
4. To start a preset, post the preset's own `ContentItem` XML to `/select`
5. To disengage streaming playback, switch to `AUX` via `/select`
6. Re-check `/now_playing`

## Known-good Bose examples

Set the target for the current shell:

```bash
SPEAKER="<speaker-host-or-ip>"
PORT="8090"
```

### Inspect the device

```bash
curl -s "http://${SPEAKER}:${PORT}/info"
curl -s "http://${SPEAKER}:${PORT}/sources"
curl -s "http://${SPEAKER}:${PORT}/presets"
curl -s "http://${SPEAKER}:${PORT}/now_playing"
curl -s "http://${SPEAKER}:${PORT}/volume"
```

### Start a Spotify-backed preset via `/select`

```bash
curl -s -X POST \
  -H 'Content-Type: text/xml' \
  --data-binary '<ContentItem source="SPOTIFY" type="uri" location="<preset-location>" sourceAccount="<account-id>"><itemName><preset-name></itemName></ContentItem>' \
  "http://${SPEAKER}:${PORT}/select"
```

### Switch to AUX as a practical stop fallback

```bash
curl -s -X POST \
  -H 'Content-Type: text/xml' \
  --data-binary '<ContentItem source="AUX" sourceAccount="AUX"></ContentItem>' \
  "http://${SPEAKER}:${PORT}/select"
```

## Follow-up work worth doing

- determine the exact working request shape for Bose `/key`
- test direct volume writes via `POST /volume`
- create a small wrapper or script for repeatable speaker actions
- extend the same documentation pattern to Chromecast or AirPlay control once a usable local control path is chosen
