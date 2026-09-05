---
name: comcast-firebolt-capability-grants
description: Check whether a Firebolt capability is usable, ask the user for it, and react when it is granted or revoked — the authorization flow that gates most of the Comcast Firebolt surface.
api: comcast:firebolt-sdk
contract: openrpc/comcast-firebolt-core-openrpc.json
contract_version: 1.7.0
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openrpc/comcast-firebolt-core-openrpc.json (Capabilities module)
  and openrpc/comcast-firebolt-manage-openrpc.json (UserGrants module), plus
  https://docs.developer.comcast.com/docs/170-core-capabilities
operations:
  - Capabilities.supported
  - Capabilities.available
  - Capabilities.permitted
  - Capabilities.granted
  - Capabilities.info
  - Capabilities.request
  - Capabilities.onAvailable
  - Capabilities.onUnavailable
  - Capabilities.onGranted
  - Capabilities.onRevoked
  - UserGrants.app
  - UserGrants.capability
  - UserGrants.grant
  - UserGrants.deny
  - UserGrants.clear
  - UserGrants.request
capabilities:
  - xrn:firebolt:capability:capabilities:info
  - xrn:firebolt:capability:capabilities:request
  - xrn:firebolt:capability:grants:state
---

# Firebolt capability grants

Every one of the 330 Firebolt methods declares the capability URNs it uses,
manages or provides. If the capability is not usable, the call fails. This is
Firebolt's authorization model — there are no OAuth scopes on this surface.
The full list of 62 capability URNs is in `scopes/comcast-scopes.yml`.

## 1. Ask one question, not four

`Capabilities.info(capabilities)` returns a `CapabilityInfo` per URN carrying
`supported`, `available`, per-role `use` / `manage` / `provide` permission
status, and a `details` array of deny reasons. Prefer it over calling
`Capabilities.supported`, `.available`, `.permitted` and `.granted` separately —
one call, one consistent answer.

## 2. Read the deny reason, then branch differently

The six `DenyReason` values are not interchangeable, and treating them as one
"denied" case is the most common mistake here:

| Reason | What it means | What to do |
| --- | --- | --- |
| `unsupported` | The device cannot do this at all | Degrade permanently. Do not prompt. |
| `unpermitted` | Your app manifest does not allow it | Not fixable at runtime. Needs re-certification. |
| `disabled` | Supported but switched off | Subscribe to `Capabilities.onAvailable`. |
| `unavailable` | Temporarily not usable | Transient — subscribe, do not poll. |
| `ungranted` | The user has not been asked | Call `Capabilities.request`. |
| `grantDenied` | The user said no | Do **not** re-prompt in a loop. |

## 3. Request only when the reason is `ungranted`

`Capabilities.request(grants)` prompts the user. Calling it when the reason is
`unsupported` or `unpermitted` wastes a prompt and cannot succeed.

## 4. Stay subscribed

`Capabilities.onGranted` and `Capabilities.onRevoked` fire when the user changes
their mind later, in platform settings, outside your app. An app that checks
grants only at launch will keep calling a method it lost access to.

## Reversibility

Grants are reversible and the contract names the operations:
`UserGrants.deny` reverses a grant, `UserGrants.clear` removes it entirely.
**No window is documented for either.** See the `reversibility:` block in
`conventions/comcast-conventions.yml` — Comcast publishes the reversal path but
not the window, which is why the reversibility grade is `documented`, not
`verified`.
