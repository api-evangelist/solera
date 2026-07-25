# Solera (solera)

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
