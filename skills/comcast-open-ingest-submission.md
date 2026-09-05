---
name: comcast-open-ingest-submission
description: Get a Comcast SAT token and submit a GMRSS content-metadata package to the Comcast Open Ingest endpoint for NBCUniversal/OTT distribution.
api: comcast:open-ingest-api
generated: '2026-09-05'
method: generated
source: >-
  Grounded in https://docs.developer.comcast.com/docs/endpoints,
  https://docs.developer.comcast.com/docs/metadata-overview,
  https://docs.developer.comcast.com/docs/validate-your-metadata-feed and
  well-known/comcast-sat-openid-configuration.json (all fetched 2026-09-05)
operations:
  - POST /oauth/token
  - POST /openingestproxy/openIngestMerlin1
---

# Comcast Open Ingest submission

Two hosts, two steps: mint a SAT token, then POST an XML metadata package.
Both are partner-gated — you need provisioned credentials and an assigned
partner identifier before any of this works.

## 1. Mint a SAT token

```
POST https://sat-prod.codebig2.net/oauth/token
x-client-id: <your_client_id>
x-client-secret: <your_client_secret>
```

Response:

```json
{
  "access_token": "<token>",
  "expires_in": 86400,
  "scope": "x1:compass:piws:read x1:compass:piws:write",
  "token_type": "Bearer"
}
```

- Tokens last **24 hours**. Cache and refresh; do not mint per request.
- The write scope you need is `x1:compass:piws:write`.
- Your SAT client is provisioned with a single `allowedPartner`. The token is
  partner-scoped as well as client-scoped — you cannot ingest for a partner you
  were not issued for.
- The authorization server also publishes
  `https://sat-prod.codebig2.net/.well-known/openid-configuration`, which names
  `token_endpoint` `/v2/ws/token.oauth2`, `client_secret_basic` /
  `client_secret_post` auth, RFC 8693 token exchange and RFC 9449 DPoP. Treat
  the discovery document as authoritative and the header form above as the
  partner-onboarding recipe. See `authentication/comcast-authentication.yml`.

## 2. Validate the feed before you send it

Run the GMRSS feed through the Metadata Validator at
https://developer.ott-highway.comcast.com/feedvalidator (up to 4 items per
check). This is the only dry run available for this surface — there is no test
mode on the ingest endpoint itself. See `sandbox/comcast-sandbox.yml`.

Golden Media RSS is MRSS over RSS 2.0 plus Comcast's Merlin extensions. Bind all
three namespaces:

- `http://search.yahoo.com/mrss/` — Media RSS
- `http://purl.org/dc/terms/` — Dublin Core Terms
- `urn:uri:merlin-gold` — the Comcast `gmrss:` extension fields

## 3. Submit

```
POST https://compass-mmpwebservice-prod.codebig2.net/openingestproxy/openIngestMerlin1?schema=1.0&form=xml&partner=<partner>
Authorization: Bearer <SAT token>
Content-Type: application/xml
```

The response is an `OpenIngestResult` XML document with per-asset outcome
status. Comcast does not publish an enumerated status/error reference for it —
parse defensively and log the whole document.

## Rules that bite

- **There is no idempotency key and no documented retraction call.** A duplicate
  POST is a duplicate submission. Rights withdrawal is expressed inside the feed
  through the `gmrss:distributionRights` validity window
  (`start=…;end=…;scheme=W3CDTF`), not by calling a delete. See the
  `reversibility:` block in `conventions/comcast-conventions.yml`, where this
  surface is the one recorded at `confidence: low` precisely because no reversal
  operation is documented.
- **The host is 401 for everything anonymous.** Probed 2026-09-05: every path on
  `compass-mmpwebservice-prod.codebig2.net`, including `/.well-known/*`, returns
  401 without a bearer token.
- **Program types are a closed set**: Movie, SeriesMaster, TvSeason, Episode,
  SportingEvent, Other, plus short-form Preview and Extra.
