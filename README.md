# TELUS (telus)

TELUS Corporation is one of Canada's three national facilities-based telecommunications carriers, operating a nationwide mobile network alongside broadband, TV, security, health (TELUS Health) and agriculture (TELUS Agriculture & Consumer Goods) businesses from its Vancouver headquarters. In the API value chain TELUS is a network owner rather than a developer-facing platform: its API Marketplace at api.telus.com sits behind the TELUS Client Identity login, its developers.telus.com "Simplify" portal is an internal/partner surface that refuses anonymous requests, and its IoT Marketplace is a login wall. The one genuinely public developer surface is the TELUS Insights Location API, a geo-intelligence product documented openly on Postman with a live OAuth2-protected gateway, though credentials are issued only through a sales onboarding process. TELUS is a named participant in the CAMARA project and appears in the CAMARA landscape as an operator, but it publishes no first-party CAMARA endpoint — its Number Verification and SIM Swap network APIs reach developers indirectly through EnStream LP, the Bell/Rogers/TELUS identity joint venture, which feeds Aduna and from there CPaaS channels such as Vonage. TELUS is partner-gated and, for network APIs, reachable only through aggregators.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/telus/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/telus/refs/heads/main/apis.yml)

## Scope

- **Kind:** company
- **Home market:** Canada
- **Tier:** Mobile network operator / broadband carrier

## Tags

- Telecommunications
- Canada
- Mobile Network Operator
- Broadband
- Network APIs
- CAMARA
- Open Gateway
- SIM Swap
- Identity Verification
- Location Intelligence
- IoT
- 5G

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

### TELUS Insights Location API

The TELUS Insights Location API exposes de-identified, aggregated geo-intelligence derived from the TELUS mobile network across Canada. Consumers submit asynchronous count jobs — demographic, origin, destination, origin-destination matrix, dwell time, trade area, repeat visitation, total trip, unique devices, home and work — against uploaded shapefile study zones or built-in geofences, then poll a job identifier for results. Documentation is published publicly through Postman; the gateway at `location-api.insights.telus.com` is live and returns 401 without a bearer token. Access uses OAuth2 client credentials issued through the TELUS Insights Portal after a sales-led onboarding, so the API is documented publicly but is not self-serve.

- **Human URL:** [https://docs.insights.telus.com/](https://docs.insights.telus.com/)
- **Base URL:** `https://location-api.insights.telus.com/product/insightsRequest/v1`

#### Properties

- [Documentation](https://docs.insights.telus.com/)
- [APIReference](https://docs.insights.telus.com/)
- [Postman Collection](collections/telus-insights-location-api.postman_collection.json)
- [Portal](https://insights.telus.com/)

### TELUS Health CHR Enterprise API

The TELUS Health Collaborative Health Record (CHR) Enterprise API is a GraphQL interface onto the CHR ambulatory-care platform used by Canadian clinics. Its published introspection document describes **505 types, 64 root queries and 49 mutations** spanning patients, appointments and availability, encounters, referrals, cases, tasks, group visits, attachments, letters, lab results (including OLIS lab messages), allergy and medication records, health profiles, facilities, providers and work hours. Pagination follows the GraphQL Cursor Connections specification (default 50, maximum 100). Authentication is a consumer-minted **RS512 JWT** — you generate a 2048-bit RSA key pair, register the public key on an API Consumer inside the CHR account, and sign your own token with a matching `iss` and a ≤ 900-second expiry. There is no authorization server, no token endpoint and no client secret. A companion Event Notification Service pushes **21 signed webhook topics** carrying identifiers only, with no personal or personal-health information in the payload.

- **Human URL:** [https://help.inputhealth.com/en/collections/3317215-chr-enterprise-api](https://help.inputhealth.com/en/collections/3317215-chr-enterprise-api)
- **Schema surface:** `https://apidocs.ca.inputhealth.com/enterprise-api` (GraphQL Voyager)

#### Properties

- [GraphQL SDL](graphql/telus-chr-enterprise-api.graphql)
- [GraphQL introspection (verbatim)](graphql/telus-chr-enterprise-api-introspection.json)
- [Documentation](https://help.inputhealth.com/en/collections/3317215-chr-enterprise-api)
- [API reference](https://help.inputhealth.com/en/articles/5941595-api-reference-documentation)
- [Getting started / onboarding](https://help.inputhealth.com/en/articles/6368814-enterprise-api-onboarding-overview)
- [Event notifications](asyncapi/telus-chr-event-notifications.yml)
- [Changelog](https://help.inputhealth.com/en/articles/13391004-what-s-new-in-the-api-2026)

## What is not here

No OpenAPI or Swagger document is published publicly by TELUS, so there is no `openapi/` directory in this repository. The Insights documentation points readers at a "Swagger file" that lives behind the Insights Portal login; every anonymous specification path probed returned 404, 401 or an SPA catch-all. There is also no MCP server, no CLI, no API client SDK in any language, no public status page, no SLA, no published deprecation policy, no idempotency contract on either API, and no TELUS-owned `security.txt`.

The one machine-readable contract TELUS does publish is the CHR GraphQL introspection document above — and it is not on a `telus.com` domain, which is why the first review round recorded "zero specs harvested."

The TELUS API Marketplace at [api.telus.com](https://api.telus.com/) redirects anonymous requests to the TELUS Client Identity login. `developers.telus.com` — the internal "Simplify" developer portal — resets anonymous TLS connections. `iot.telus.com` is an IoT Marketplace login page. `opengateway.telus.com`, `developer.telus.com`, `apis.telus.com` and `networkapi.telus.com` do not resolve. `designsystem.telus.com` and `tds.telus.com` serve expired TLS certificates.

## CAMARA and GSMA Open Gateway posture

TELUS participates in [CAMARA](https://camaraproject.org/) — six TELUS staff are named in `camaraproject/Governance/PARTICIPANTS.MD`, and TELUS is listed as an Operator in the CAMARA landscape — but it exposes no first-party CAMARA endpoint. Canada's network APIs are aggregated by **EnStream LP**, the mobile-identity joint venture owned by Bell Mobility, Rogers and TELUS, which partnered with **Aduna** (the Ericsson-led carrier JV) in February 2025 to publish **Number Verification** and **SIM Swap**. In June 2026 **Vonage** commercially launched SIM Swap Detection and Silent Authentication in Canada over that same path. A developer who wants a TELUS network capability buys it from Vonage or Aduna — never from TELUS.

No evidence was found that TELUS signed the GSMA Open Gateway MoU, and no TM Forum Open API conformance certification was found. CIBA, the backchannel authentication flow CAMARA specifies, does not appear on any TELUS property.

## Artifacts in this repository

| Artifact | Path |
| --- | --- |
| GraphQL SDL + introspection (CHR) | `graphql/` |
| Postman collection (Insights) | `collections/` |
| Authentication profile | `authentication/telus-authentication.yml` |
| OAuth scopes | `scopes/telus-scopes.yml` |
| API conventions | `conventions/telus-conventions.yml` |
| Error catalog | `errors/telus-problem-types.yml` |
| Rate limits and usage guidance | `rate-limits/telus-rate-limits.yml` |
| Webhook / event catalog (CHR ENS) | `asyncapi/telus-chr-event-notifications.yml` |
| Lifecycle and versioning | `lifecycle/telus-lifecycle.yml` |
| Changelog | `changelog/telus-changelog.yml` |
| Data model | `data-model/telus-data-model.yml` |
| Conformance | `conformance/telus-conformance.yml` |
| Sandbox and test surfaces | `sandbox/telus-sandbox.yml` |
| Packages | `packages/telus-packages.yml` |
| Embedded components (UDS) | `components/telus-components.yml` |
| Agent skills | `skills/` |
| llms.txt | `llms/telus-llms.txt` |
| Well-known discovery probes | `well-known/telus-well-known.yml` |
| Domain security | `security/telus-domain-security.yml` |
| Vulnerability disclosure | `security/telus-vulnerability-disclosure.yml` |

## Links

- [TELUS](https://www.telus.com/)
- [TELUS Insights API documentation](https://docs.insights.telus.com/)
- [TELUS API Marketplace (login required)](https://api.telus.com/)
- [API Marketplace Support](https://support.api.telus.com/)
- [EnStream](https://enstream.com/)
