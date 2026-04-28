# Comcast (comcast)

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
