---
name: comcast-firebolt-media-metrics
description: Report app and playback telemetry to the Comcast platform through the Firebolt Metrics module, in the order and with the events the platform expects.
api: comcast:firebolt-sdk
contract: openrpc/comcast-firebolt-core-openrpc.json
contract_version: 1.7.0
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openrpc/comcast-firebolt-core-openrpc.json (Metrics module, 20
  methods) and https://docs.developer.comcast.com/docs/implementing-firebolt-metrics
operations:
  - Metrics.ready
  - Metrics.appInfo
  - Metrics.signIn
  - Metrics.signOut
  - Metrics.page
  - Metrics.action
  - Metrics.error
  - Metrics.startContent
  - Metrics.stopContent
  - Metrics.mediaLoadStart
  - Metrics.mediaPlay
  - Metrics.mediaPlaying
  - Metrics.mediaPause
  - Metrics.mediaWaiting
  - Metrics.mediaProgress
  - Metrics.mediaSeeking
  - Metrics.mediaSeeked
  - Metrics.mediaRateChange
  - Metrics.mediaRenditionChange
  - Metrics.mediaEnded
capabilities:
  - xrn:firebolt:capability:metrics:general
  - xrn:firebolt:capability:metrics:media
  - xrn:firebolt:capability:metrics:distributor
---

# Firebolt media and app metrics

Metrics are not optional instrumentation on this platform. The Error Free
Session Rate and Time To Minimally Usable numbers that gate staged deployment
(`lifecycle/comcast-lifecycle.yml`) are computed from what you report here.

## App-level events

- `Metrics.ready()` — the app is usable. Pair it with `Lifecycle.ready()`.
- `Metrics.appInfo(build)` — identify your build.
- `Metrics.page(pageId)` — a screen was shown.
- `Metrics.action(category, type, parameters)` — a user did something.
- `Metrics.signIn()` / `Metrics.signOut()` — entitlement state changed.
- `Metrics.error(type, code, description, visible, parameters)` — something
  failed. `type` is the contract's `ErrorType` enum: `network`, `media`,
  `restriction`, `entitlement`, `other`. Pick the right one; these are the five
  values in `errors/comcast-problem-types.yml`, and they are what the platform
  aggregates on.

## Playback events, in order

`Metrics.startContent(entityId)` opens a playback session, then the media events
follow the actual player state:

`mediaLoadStart` → `mediaPlay` → `mediaPlaying` → (`mediaWaiting`, `mediaPause`,
`mediaSeeking`/`mediaSeeked`, `mediaRateChange`, `mediaRenditionChange`,
`mediaProgress`) → `mediaEnded`

Close with `Metrics.stopContent(entityId)`.

- `mediaProgress` is periodic; the others are transitions.
- `mediaWaiting` is the rebuffer signal. Under-reporting it makes your app look
  better than it is and will not survive the certification lab.
- `mediaRenditionChange` carries the bitrate/profile switch — report it or ABR
  behaviour is invisible in the platform's data.

## Rules that bite

- **Privacy gates telemetry.** `Privacy.allowProductAnalytics`,
  `Privacy.allowWatchHistory` and `Privacy.allowACRCollection` are user
  settings on the Manage SDK. Read the capability state first; do not assume
  reporting is permitted.
- **Metrics calls are fire-and-forget with no idempotency key.** A retried
  `mediaPlay` is a second `mediaPlay`. See
  `conventions/comcast-conventions.yml`.
- **No error codes are declared** for any Metrics method. Fail soft: telemetry
  must never break playback.
