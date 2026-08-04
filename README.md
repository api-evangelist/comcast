# Comcast (comcast)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Comcast Corporation is a global media and technology company with two primary businesses, Comcast Cable (Xfinity) and NBCUniversal, providing video, internet, voice, wireless, and entertainment services to residential and business customers. Comcast publishes a public developer program centered on the Firebolt application platform for connected TV experiences, along with authentication and content ingest endpoints used by NBCUniversal media partners. The Firebolt SDK family is used by app developers to write apps once and deploy across Xfinity X1, Xfinity Flex, Sky Q, and other Comcast set-top boxes and connected devices.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/comcast/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Cable
- Connected Devices
- Entertainment
- Internet
- Media
- Mobile
- Streaming
- Wireless

## Timestamps

- **Created:** 2026-03-21
- **Modified:** 2026-04-26

## APIs

### Comcast Firebolt SDK
Firebolt is Comcast's application platform for building apps that run on TVs, set-top boxes, and other connected home devices. The Firebolt SDK exposes a family of JavaScript APIs (Lifecycle, Metrics, Device, Authentication, Discovery, Profile, Localization, Account) defined with OpenRPC and distributed via npm.

**Human URL:** [https://docs.developer.comcast.com/docs/firebolt-apis](https://docs.developer.comcast.com/docs/firebolt-apis)

#### Tags

- Connected Devices, JavaScript, SDK, Set-Top Box, TV Apps

#### Properties

- [Documentation](https://docs.developer.comcast.com/docs/firebolt-apis)
- [Getting Started](https://docs.developer.comcast.com/docs/intro-to-firebolt)
- [npm Package](https://www.npmjs.com/package/@firebolt-js/sdk)
- [Repository](https://github.com/rdkcentral/firebolt-openrpc)

### Comcast Authentication API (SAT)
Issues short-lived bearer tokens used to authenticate calls to Comcast partner APIs (such as Open Ingest). Clients exchange an x-client-id and x-client-secret for an access token valid for 24 hours.

**Human URL:** [https://docs.developer.comcast.com/docs/081-core-authentication](https://docs.developer.comcast.com/docs/081-core-authentication)

#### Tags

- Authentication, OAuth, Tokens

#### Properties

- [Documentation](https://docs.developer.comcast.com/docs/081-core-authentication)

### Comcast Open Ingest API
NBCUniversal media partner endpoint that accepts XML metadata and content asset packages, authenticated with a Comcast SAT bearer token and partner-scoped (e.g. globalott).

**Human URL:** [https://docs.developer.comcast.com/docs/endpoints](https://docs.developer.comcast.com/docs/endpoints)

#### Tags

- Ingest, Media, Metadata, NBCUniversal

#### Properties

- [Documentation](https://docs.developer.comcast.com/docs/endpoints)

## Common Properties

- [Website](https://www.comcast.com)
- [Developer Docs](https://docs.developer.comcast.com/)
- [Developer Portal](https://developer.comcast.com/)
- [GitHub](https://github.com/Comcast)
- [Open Source](https://comcast.github.io/)
- [Xfinity](https://www.xfinity.com/)
- [NBCUniversal](https://www.nbcuniversal.com/)
- [Investors](https://www.cmcsa.com/)
- [Privacy](https://www.xfinity.com/privacy/policy)
- [Terms](https://developers.xfinity.com/TOS.html)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
