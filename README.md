# Neurable

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

Neurable is a Boston, Massachusetts neurotechnology company, founded in 2015 on University of Michigan EEG signal-processing research, that builds non-invasive brain-computer interface (BCI) hardware and the AI models that interpret the signal. Its consumer product is the MW75 Neuro — Master & Dynamic over-ear headphones with dry conductive-fabric EEG electrodes that measure focus, fatigue and cognitive load — and it also ships an MW75 Neuro Research Kit (12-channel, 500 Hz raw EEG plus accelerometer/gyroscope, exported for LSL and BrainVision toolchains) and a partner Development Kit for hardware and software OEMs embedding Neurable sensing into their own devices. Neurable publishes no public developer portal or API reference, but its production backend services expose FastAPI-generated OpenAPI 3.1.0 descriptions at their host roots: an Analytics Service handling the EEG recording lifecycle and headset feature licensing, a real-time data processing pipe service that is also a full OpenID Connect authorization server, and a Brain Health Service.

**Areas:** Neurotechnology · Brain-Computer Interface · EEG · Neuroscience · Wearables · biosignals · Hardware · Consumer Electronics · Research Tools · cognitive-analytics · Health Data · Authentication

## Neurable on APIs.io

Everything profiled for Neurable is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/neurable/](https://apis.io/providers/neurable/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [Neurable Core API](https://apis.io/apis/neurable/neurable-core-api/) — The Core API from Neurable — 2 operation(s) for core.
- [Neurable OIDC API](https://apis.io/apis/neurable/neurable-oidc-api/) — The OIDC API from Neurable — 2 operation(s) for oidc.
- [Neurable Open API](https://apis.io/apis/neurable/neurable-open-api/) — The open API from Neurable — 1 operation(s) for open.
- [Neurable Protected API](https://apis.io/apis/neurable/neurable-protected-api/) — The protected API from Neurable — 5 operation(s) for protected.
- [Neurable O Auth API](https://apis.io/apis/neurable/neurable-oauth-api/) — The OAuth API from Neurable — 3 operation(s) for oauth.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [neurable-mcp.yml](https://apis.io/servers/neurable/neurable-mcp-yml/)

#### Collections (8)

Postman and Open Collection request collections.

- [Analytics Service](https://apis.io/collections/neurable/open-neurable-analytics-service/)
- [Brain Health Service](https://apis.io/collections/neurable/open-neurable-brain-health-service/)
- [neu-pipe-service Core API](https://apis.io/collections/neurable/open-neurable-core-api/)
- [neu-pipe-service O Auth API](https://apis.io/collections/neurable/open-neurable-oauth-api/)
- [neu-pipe-service OIDC API](https://apis.io/collections/neurable/open-neurable-oidc-api/)
- [Analytics Service Open API](https://apis.io/collections/neurable/open-neurable-open-api/)
- [neu-pipe-service](https://apis.io/collections/neurable/open-neurable-pipe-service/)
- [Analytics Service Protected API](https://apis.io/collections/neurable/open-neurable-protected-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Neurable Analytics Service Overlay](https://apis.io/overlays/neurable/neurable-analytics-service-overlay/)
- [Neurable Pipe Service Overlay](https://apis.io/overlays/neurable/neurable-pipe-service-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Neurable Authentication](https://apis.io/security/neurable/neurable-authentication/)
- [Neurable Domain Security](https://apis.io/security/neurable/neurable-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Neurable Scopes](https://apis.io/scopes/neurable/neurable-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.neurable.com/)
- [Support](https://www.neurable.com/contact)
- [Blog](https://www.neurable.com/blog)
- [Terms of service](https://www.neurable.com/terms-of-service)
- [Privacy policy](https://www.neurable.com/privacy-policy)
- [GitHub](https://github.com/neurable)

## API Evangelist rating

**Kin Score 32.5** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 39.6 |
| Contract governance | 18.2 |
| Operational transparency | 2.6 |
| Developer ergonomics | 28.0 |
| Access clarity | 21.1 |

**Agent readiness 30.4** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 32.8** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/neurable/](https://apis.io/providers/neurable/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Neurable: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/neurable/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
