# TELUS

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

TELUS Corporation is one of Canada's three national facilities-based telecommunications carriers, operating a nationwide mobile network alongside broadband, TV, security, health (TELUS Health) and agriculture (TELUS Agriculture & Consumer Goods) businesses from its Vancouver headquarters. In the API value chain TELUS is a network owner rather than a developer-facing platform: its API Marketplace at api.telus.com sits behind the TELUS Client Identity login, its developers.telus.com "Simplify" portal is an internal/partner surface that refuses anonymous requests, and its IoT Marketplace is a login wall. Two TELUS APIs are nonetheless documented in the open, and neither is on a telus.com property. The TELUS Insights Location API publishes a 27-operation reference through Postman Documenter and runs a live OAuth2-protected gateway. The TELUS Health Collaborative Health Record (CHR) Enterprise API is a substantial GraphQL platform — 505 types, 64 queries, 49 mutations and a 21-topic signed webhook service — whose full introspection document is anonymously fetchable from apidocs.ca.inputhealth.com, making it the only machine-readable API contract TELUS publishes anywhere. Both are gated commercially: Insights credentials come from a sales-led ticket in the Insights Portal, and CHR Enterprise API access is a paid feature enabled per clinic account after a contract and a privacy questionnaire. TELUS is a named participant in the CAMARA project and appears in the CAMARA landscape as an operator, but it publishes no first-party CAMARA endpoint — its Number Verification and SIM Swap network APIs reach developers indirectly through EnStream LP, the Bell/Rogers/TELUS identity joint venture, which feeds Aduna and from there CPaaS channels such as Vonage. TELUS is partner-gated and, for network APIs, reachable only through aggregators.

**Areas:** Telecommunications · Canada · Mobile Network Operator · Broadband · Network APIs · CAMARA · Open Gateway · SIM Swap · Identity Verification · Location Intelligence · IoT · 5G

## TELUS on APIs.io

Everything profiled for TELUS is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/telus/](https://apis.io/providers/telus/)** — or go straight to one of the APIs or properties below.

### APIs (2)

- [TELUS Insights Location API](https://apis.io/apis/telus/telus-insights-location-api/) — The TELUS Insights Location API exposes de-identified, aggregated geo-intelligence derived from the TELUS mobile network across Canada. Consumers submit…
- [TELUS Health CHR Enterprise API](https://apis.io/apis/telus/telus-health-chr-enterprise-api/) — The TELUS Health Collaborative Health Record (CHR) Enterprise API is a GraphQL interface onto the CHR ambulatory-care platform used by Canadian clinics and…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Telus Chr Event Notifications](https://apis.io/asyncapis/telus/telus-chr-event-notifications/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Telus Authentication](https://apis.io/security/telus/telus-authentication/)
- [Telus Domain Security](https://apis.io/security/telus/telus-domain-security/)
- [Telus Vulnerability Disclosure](https://apis.io/security/telus/telus-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Telus Scopes](https://apis.io/scopes/telus/telus-scopes/)

#### Rate limits (1)

Published request limits.

- [Telus Rate Limits](https://apis.io/rate-limits/telus/telus-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.telus.com/)
- [Documentation](https://docs.insights.telus.com/)
- [Getting started](https://help.inputhealth.com/en/articles/6368814-enterprise-api-onboarding-overview)
- [API reference](https://help.inputhealth.com/en/articles/5941595-api-reference-documentation)
- [Support](https://support.api.telus.com/)
- [Blog](https://www.telus.com/en/blog)
- [Security](https://www.telus.com/en/about/security/report-a-problem)
- [Terms of service](https://www.telus.com/en/about/policies-and-disclosures/user-terms)
- [Privacy policy](https://www.telus.com/en/about/privacy/commitment)
- [GitHub](https://github.com/telus)

## API Evangelist rating

**Kin Score 50.4** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 51.0 |
| Contract governance | 4.5 |
| Operational transparency | 68.4 |
| Developer ergonomics | 68.5 |
| Access clarity | 21.1 |

**Agent readiness 23.9** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 45.0** — Telecommunications

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/telus/](https://apis.io/providers/telus/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for TELUS: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/telus/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
