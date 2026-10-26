# Solera (solera)

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

Solera is a Westlake, Texas headquartered vehicle lifecycle management software, data, and services company operating in more than 100 countries, and one of the claims-technology intermediaries that sits between property and casualty insurers and the repair, salvage, and fleet economy rather than underwriting risk itself. Its insurance-facing business is automobile physical damage claims — first notice of loss intake, photo and AI damage assessment, repair cost estimating, total loss valuation, parts sourcing, and claims workflow — delivered through the Audatex and Qapter brands alongside Vehicle Repair, Dealer Solutions, and Fleet Solutions divisions.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/solera/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/solera/refs/heads/main/apis.yml)

## API Posture

Solera has **no self-serve developer portal**. Every first-party developer subdomain probed on 2026-07-25 — `developer.solera.com`, `developers.solera.com`, `docs.solera.com`, `api.solera.com`, `apis.solera.com` — fails to resolve, and `/developers`, `/developer`, `/api`, `/partners` and `/integrations` on `www.solera.com` all return HTTP 404.

The Solera Integrations portal at [https://na.api.solera.com/](https://na.api.solera.com/) returns HTTP 200 but redirects straight to `/Login` and renders an Audatex-branded User ID and Password sign-in form. There is no registration, no plan page, and no public API key. That is a **partner login wall**, not a developer portal, and onboarding is a provisioned B2B integration run through a Solera account representative with a kickoff meeting, a segregated client test environment, and UAT.

What is genuinely public is narrower but real: three anonymously readable Swagger UI reference pages and three machine-readable **OpenAPI 3.0.1** documents on the Audatex demo API host, plus PDF and DOCX integration guides served without login from `na.api.solera.com/files/`. All three specs were harvested verbatim into [`openapi/`](openapi/).

### Standards posture

**No ACORD reference found.** Nothing on solera.com, na.api.solera.com, in the harvested specs, or in the integration guides mentions ACORD, AL3, ACORD XML, NGDS, IVANS, agency download, Applied Epic, or Vertafore AMS360. Solera's published standards alignment is **CIECA** — the auto physical damage sector's analogue to ACORD. The Estimate Document Return API Integration Guide states that Audatex is a Corporate Technology member of the Collision Industry Electronic Commerce Association and is licensed to use CIECA standards in its products, with **BMS 5.7** as the schema baseline for estimate return data mapping.

### Insurance verbs

| Verb | Exposed | Notes |
| --- | --- | --- |
| Quote | No | Solera is not a carrier; there is no rating or quoting surface. |
| Bind | No | Not applicable. |
| Issue | No | Not applicable. |
| **FNOL** | **Yes (partner-only)** | Assignment creation and estimate return; OAuth scopes `b2b.fnol.api`, `gofnol.api`, `b2b.fnol.documents`, `b2b.fnol.transformer`. |

### Authentication

OAuth 2.0 via IdentityServer, with Basic Authorization also documented. The OpenID Connect discovery document at [dispatch-login-demo.audatex.com/.well-known/openid-configuration](https://dispatch-login-demo.audatex.com/.well-known/openid-configuration) is anonymously readable and advertises `authorization_code`, `client_credentials`, `password`, `refresh_token`, `implicit`, and device-code grants across 32 scopes including `estimatics.api`, `hqclaims.api`, `novo.claims.manager`, `mobile.inspection.api`, `marketdata.web.api`, and `vinhistory.api`.

## Tags

- Insurance
- United States
- Property and Casualty
- Claims
- Claims Technology
- Automotive Claims
- FNOL
- Vehicle Damage Assessment
- Risk Data
- CIECA
- Insurtech

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

### Solera Dashboard Assignment API

Assignment dispatch and first notice of loss intake for automobile physical damage claims. Creates a new assignment, retrieves a sample assignment request message, posts assignment acknowledgements, and generates an estimate return response.

- **Human URL:** [https://api-demo.audatex.com/TestAssignmentapi/docs/index.html](https://api-demo.audatex.com/TestAssignmentapi/docs/index.html)
- **Base URL:** `https://api-demo.audatex.com/TestAssignmentapi`

#### Properties

- [OpenAPI](openapi/solera-dashboard-assignment-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://na.api.solera.com/files/Estimate%20Return%20API.pdf)
- [API Reference](https://api-demo.audatex.com/TestAssignmentapi/docs/index.html)
- [Authentication](https://dispatch-login-demo.audatex.com/.well-known/openid-configuration)

### Solera ClaimImages API

Retrieval of claim images and decoded document files attached to an automobile damage claim, used by insurer claim management systems consuming Audatex claim documentation.

- **Human URL:** [https://api-demo.audatex.com/TestClaimImageapi/docs/index.html](https://api-demo.audatex.com/TestClaimImageapi/docs/index.html)
- **Base URL:** `https://api-demo.audatex.com/TestClaimImageapi`

#### Properties

- [OpenAPI](openapi/solera-claim-images-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://na.api.solera.com/files/ClaimImage%20Return%20API.pdf)
- [API Reference](https://api-demo.audatex.com/TestClaimImageapi/docs/index.html)
- [Authentication](https://dispatch-login-demo.audatex.com/.well-known/openid-configuration)

### Solera EAPI GIC Integration API

Global Integration Component endpoint used to post GIC integration payloads against a work assignment identifier, acknowledge M31 events, and report the deployed API version.

- **Human URL:** [https://api-demo.audatex.com/TestGICapi/docs/index.html](https://api-demo.audatex.com/TestGICapi/docs/index.html)
- **Base URL:** `https://api-demo.audatex.com/TestGICapi`

#### Properties

- [OpenAPI](openapi/solera-gic-integration-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://na.api.solera.com/files/GIC%20-%20Image%20Capture%20API.docx)
- [API Reference](https://api-demo.audatex.com/TestGICapi/docs/index.html)
- [Authentication](https://dispatch-login-demo.audatex.com/.well-known/openid-configuration)

## Links

- [Website](https://www.solera.com/)
- [Company](https://www.solera.com/company/)
- [Blog](https://www.solera.com/blog/)
- [Solera Integrations Portal (login wall)](https://na.api.solera.com/)
- [CIECA](https://www.cieca.com/)

## Review

See [review.yml](review.yml) for the full API Evangelist review, including every probed URL with its HTTP status, spec provenance, the verbatim CIECA statement, and the OAuth scope inventory.

## Related

Audatex is a Solera company and has a separate profile at [api-evangelist/audatex](https://github.com/api-evangelist/audatex), which overlaps this surface.
