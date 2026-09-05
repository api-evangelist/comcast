---
name: comcast-firebolt-app-lifecycle
description: Bring a Firebolt app through launch, foreground/background transitions and shutdown on a Comcast X1 or Xfinity device, using only methods that exist in the Firebolt Core 1.7.0 contract.
api: comcast:firebolt-sdk
contract: openrpc/comcast-firebolt-core-openrpc.json
contract_version: 1.7.0
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openrpc/comcast-firebolt-core-openrpc.json (Lifecycle module,
  9 methods) and https://docs.developer.comcast.com/docs/lifecycle-management
operations:
  - Lifecycle.ready
  - Lifecycle.state
  - Lifecycle.close
  - Lifecycle.finished
  - Lifecycle.onForeground
  - Lifecycle.onBackground
  - Lifecycle.onInactive
  - Lifecycle.onSuspended
  - Lifecycle.onUnloading
capabilities:
  - xrn:firebolt:capability:lifecycle:initialize
  - xrn:firebolt:capability:lifecycle:ready
  - xrn:firebolt:capability:lifecycle:state
  - xrn:firebolt:capability:lifecycle:launch
---

# Firebolt app lifecycle

The platform, not the app, decides when an app runs. Firebolt tells the app what
state it is in; the app's job is to answer promptly and to stop doing work it is
not allowed to do.

All calls are JSON-RPC 2.0. Import from `@firebolt-js/sdk` (npm, 1.7.0).

## 1. Signal you are ready

Call `Lifecycle.ready()` as soon as the first frame the user can see is drawn —
not when your code finishes booting. The platform measures Time To Minimally
Usable from launch to this call, and that number gates staged deployment (see
`lifecycle/comcast-lifecycle.yml`).

## 2. Subscribe before you need to react

Register every state listener during initialization, not lazily:

- `Lifecycle.onForeground` — you are visible and interactive.
- `Lifecycle.onBackground` — you are still running but not visible. Stop
  rendering, stop playback, keep state.
- `Lifecycle.onInactive` — you have been launched but not brought forward.
- `Lifecycle.onSuspended` — the platform may reclaim your memory.
- `Lifecycle.onUnloading` — you are about to be torn down. Persist anything you
  need with `SecureStorage.set` now; there is no later.

`Lifecycle.state()` reads the current state on demand, but events are the
contract: polling state is not how this API is meant to be used.

## 3. Exit cleanly

- `Lifecycle.close(reason)` asks the platform to move you out of the foreground.
- `Lifecycle.finished()` tells the platform you have completed your unload work.
  Call it after `onUnloading`, or the platform will tear you down on its own
  schedule.

## Rules that bite

- **190MB RAM cap.** Exceeding it is a deployment blocker, not a warning.
  Verify with the Memory Monitor overlay
  (https://docs.developer.comcast.com/docs/memory-monitor).
- **The SDK does not validate your parameters.** A malformed call fails at
  runtime on the device. Rehearse against Mock Firebolt
  (`sandbox/comcast-sandbox.yml`), which does validate.
- **No idempotency mechanism exists.** See `conventions/comcast-conventions.yml`
  — there is no Idempotency-Key on any Comcast surface. Lifecycle methods are
  state transitions; do not retry blindly across a state change.
- **Errors are not in the contract.** Zero of the 330 Firebolt methods declare
  an `errors` array. Handle failure generically from the JSON-RPC error object
  and consult `errors/comcast-problem-types.yml` for the deny taxonomy.
