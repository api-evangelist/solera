# Solera

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

Solera is a Westlake, Texas headquartered vehicle lifecycle management software, data, and services company operating in more than 100 countries, and one of the claims-technology intermediaries that sits between property and casualty insurers and the repair, salvage, and fleet economy rather than underwriting risk itself. Its insurance-facing business is automobile physical damage claims — first notice of loss intake, photo and AI damage assessment, repair cost estimating, total loss valuation, parts sourcing, and claims workflow — delivered through the Audatex and Qapter brands alongside Vehicle Repair (Identifix, Autodata, Hollander), Dealer Solutions (DealerSocket, cap hpi), and Fleet Solutions (Omnitracs, SmartDrive, eDriving) divisions. Its API posture is characteristic of the United States insurance market seam — there is no self-serve developer portal on solera.com, no signup, and no public API key. The Solera Integrations portal at na.api.solera.com returns HTTP 200 to an Audatex user ID and password login wall, and onboarding is a provisioned B2B integration run through a Solera account representative with a kickoff meeting and UAT. What is genuinely public is a set of anonymously readable Swagger UI reference pages and machine readable OpenAPI 3.0.1 documents on the Audatex demo API host, plus PDF integration guides on na.api.solera.com/files, covering assignment dispatch and first notice of loss, claim image retrieval, and GIC integration. Authentication is OAuth 2.0 against an IdentityServer discovery document at dispatch-login-demo.audatex.com with resource scopes including b2b.fnol.api, gofnol.api, estimatics.api, and hqclaims.api. Solera does not reference ACORD anywhere in its public integration documentation. Its published standards alignment is CIECA instead — Audatex is documented as a Corporate Technology member of the Collision Industry Electronic Commerce Association and licensed to use CIECA standards, with the BMS 5.7 schema as the data mapping baseline for estimate return, which is the auto physical damage sector's analogue to ACORD rather than ACORD itself.

**Areas:** Insurance · United States · Property and Casualty · Claims · Claims Technology · Automotive Claims · FNOL · Vehicle Damage Assessment · Risk Data · CIECA · Insurtech

## Solera on APIs.io

Everything profiled for Solera is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/solera/](https://apis.io/providers/solera/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [Solera Assignment API](https://apis.io/apis/solera/solera-assignment-api/) — The Assignment API from Solera — 3 operation(s) for assignment.
- [Solera Estimate Return API](https://apis.io/apis/solera/solera-estimatereturn-api/) — The EstimateReturn API from Solera — 1 operation(s) for estimatereturn.
- [Solera Get Claim Image Response API](https://apis.io/apis/solera/solera-getclaimimageresponse-api/) — The GetClaimImageResponse API from Solera — 1 operation(s) for getclaimimageresponse.
- [Solera Get Document API](https://apis.io/apis/solera/solera-getdocument-api/) — The GetDocument API from Solera — 2 operation(s) for getdocument.
- [Solera Get Documents API](https://apis.io/apis/solera/solera-getdocuments-api/) — The GetDocuments API from Solera — 2 operation(s) for getdocuments.
- [Solera Get Image API](https://apis.io/apis/solera/solera-getimage-api/) — The GetImage API from Solera — 4 operation(s) for getimage.
- [Solera Get Valuation API](https://apis.io/apis/solera/solera-getvaluation-api/) — The GetValuation API from Solera — 1 operation(s) for getvaluation.
- [Solera Get Valuation Response API](https://apis.io/apis/solera/solera-getvaluationresponse-api/) — The GetValuationResponse API from Solera — 1 operation(s) for getvaluationresponse.
- [Solera GIC Integration API](https://apis.io/apis/solera/solera-gicintegration-api/) — The GICIntegration API from Solera — 2 operation(s) for gicintegration.
- [Solera Valuation Return API](https://apis.io/apis/solera/solera-valuationreturn-api/) — The ValuationReturn API from Solera — 2 operation(s) for valuationreturn.
- [Solera Version API](https://apis.io/apis/solera/solera-version-api/) — The Version API from Solera — 2 operation(s) for version.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (2)

Event-driven and webhook contracts.

- [Solera / Audatex EAPI Event Callbacks](https://apis.io/asyncapis/solera/solera-eapi-asyncapi/)
- [Solera Webhooks](https://apis.io/asyncapis/solera/solera-webhooks/)

#### Event channels (4)

Individual events and webhooks the provider emits.

- [{clientAssignmentAckEndpoint}](https://apis.io/channels/solera/clientassignmentackendpoint/)
- [{clientClaimImageEndpoint}](https://apis.io/channels/solera/clientclaimimageendpoint/)
- [{clientEstimateReturnEndpoint}](https://apis.io/channels/solera/clientestimatereturnendpoint/)
- [{clientM31Endpoint}](https://apis.io/channels/solera/clientm31endpoint/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [solera](https://apis.io/servers/solera/solera/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Solera Agentic Access](https://apis.io/agentic-access/solera/solera-agentic-access/)

#### Collections (10)

Postman and Open Collection request collections.

- [ClaimImages API](https://apis.io/collections/solera/open-solera-claim-images/)
- [ClaimImages API](https://apis.io/collections/solera/open-solera-claim-images-prod-swagger/)
- [Solera Dashboard Assignment API](https://apis.io/collections/solera/open-solera-dashboard-assignment/)
- [EAPI Get Document API](https://apis.io/collections/solera/open-solera-eapi-getdocument/)
- [Solera Enterprise API](https://apis.io/collections/solera/open-solera-enterprise-assignment-prod-swagger/)
- [Audatex GetDocuments API](https://apis.io/collections/solera/open-solera-getdocuments-v1/)
- [Audatex GetDocuments API](https://apis.io/collections/solera/open-solera-getdocuments-v2/)
- [Audatex Assignment Get Document API](https://apis.io/collections/solera/open-solera-getimage-v1/)
- [Audatex Assignment Get Document API](https://apis.io/collections/solera/open-solera-getimage-v2/)
- [EAPI GIC Integration API](https://apis.io/collections/solera/open-solera-gic-integration/)

#### Overlays (10)

OpenAPI overlays, including conversational phrasing for each operation.

- [Solera Claim Images Overlay](https://apis.io/overlays/solera/solera-claim-images-overlay/)
- [Solera Claim Images Prod Swagger Overlay](https://apis.io/overlays/solera/solera-claim-images-prod-swagger-overlay/)
- [Solera Dashboard Assignment Overlay](https://apis.io/overlays/solera/solera-dashboard-assignment-overlay/)
- [Solera Eapi Getdocument Overlay](https://apis.io/overlays/solera/solera-eapi-getdocument-overlay/)
- [Solera Enterprise Assignment Prod Swagger Overlay](https://apis.io/overlays/solera/solera-enterprise-assignment-prod-swagger-overlay/)
- [Solera Getdocuments V1 Overlay](https://apis.io/overlays/solera/solera-getdocuments-v1-overlay/)
- [Solera Getdocuments V2 Overlay](https://apis.io/overlays/solera/solera-getdocuments-v2-overlay/)
- [Solera Getimage V1 Overlay](https://apis.io/overlays/solera/solera-getimage-v1-overlay/)
- [Solera Getimage V2 Overlay](https://apis.io/overlays/solera/solera-getimage-v2-overlay/)
- [Solera Gic Integration Overlay](https://apis.io/overlays/solera/solera-gic-integration-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Solera Authentication](https://apis.io/security/solera/solera-authentication/)
- [Solera Domain Security](https://apis.io/security/solera/solera-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Solera Scopes](https://apis.io/scopes/solera/solera-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.solera.com/)
- [Documentation](https://na.api.solera.com/files/Estimate%20Return%20API.pdf)
- [Getting started](https://na.api.solera.com/files/Estimate%20Return%20API.pdf)
- [API reference](https://api-prod.audatex.com/TestAssignmentapi/docs/index.html)
- [Support](https://www.solera.com/contact-us/)
- [Blog](https://www.solera.com/blog/)
- [Terms of service](https://www.solera.com/us-canada-master-services-agreement/)
- [Privacy policy](https://www.solera.com/privacy-policy/)

## API Evangelist rating

**Kin Score 41.6** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 72.3 |
| Contract quality | 45.0 |
| Contract governance | 18.2 |
| Operational transparency | 23.7 |
| Developer ergonomics | 56.5 |
| Access clarity | 21.1 |

**Agent readiness 30.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | — |

**Regulatory alignment 36.8** — Insurance

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/solera/](https://apis.io/providers/solera/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Solera: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/solera/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
