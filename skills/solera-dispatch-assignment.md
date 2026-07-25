---
name: Dispatch an assignment to Audatex
description: Submit an automobile physical damage claim assignment (FNOL dispatch) to Audatex as a CIECA BMS 5.7 document, and register the callback routes Audatex will use to return the estimate and claim images.
api: openapi/solera-enterprise-assignment-prod-swagger.json
operations:
  - AddAssignment
  - AssignmentRequestMessage
  - AssignmentAcks
generated: '2026-07-25'
method: generated
source: openapi/solera-enterprise-assignment-prod-swagger.json, openapi/solera-dashboard-assignment-openapi.json, Estimate Document Return API Integration Guide, Claim Image Document Return API Integration Guide section 6.3.2
---

# Dispatch an assignment to Audatex

This is the entry point of the whole Solera / Audatex claims integration. Everything that
comes back later — estimate documents, claim images, valuations — is routed using the
callback configuration you supply *here*.

## Prerequisites

- A bearer token with scope `b2b.fnol.api` (see `solera-authenticate.md`).
- Your CIECA BMS 5.7 `AssignmentAddRq` XML document, Base64 encoded.
- A client-hosted HTTPS endpoint per callback type, and an authorization server of your
  own that Audatex can authenticate against.

## Steps

1. **Fetch the expected shape first.** Call `AssignmentRequestMessage`
   (`GET /api/v2/assignmentRequestMessage`) — it returns a live sample assignment message.
   Diff your payload against it before you ever POST a real claim.

2. **Build the request body.** The schema is `AddAssignmentRequest`:
   - `header.messageType` — `Audatex.Assignment`
   - `body.bmsVer` — `5.7.0`
   - `body.claimNumber` — your claim number
   - `body.content` — the Base64-encoded BMS `AssignmentAddRq`
   - `responseRoute` — see step 3

3. **Register your callback routes in `responseRoute`.** This is the part integrators get
   wrong:
   - `responseRoute.authorization.href` — your token endpoint
   - `responseRoute.authorization.body.client_Id` / `client_Secret` / `audience` / `scope`
     — the credentials **Audatex** will use to authenticate against **you**
   - `responseRoute.responseMessage[]` — one entry per callback, with `type` and `href`:
     - `Assignment` — assignment acknowledgement
     - `EstimateReturn` — estimate complete message
     - `ClaimImageReturn` — claim image message
   - If you omit `ClaimImageReturn`, Audatex reuses the `EstimateReturn` URL.
   - In production, endpoints and authentication are normally pre-configured with Audatex;
     in test you must supply everything in the request.

4. **POST it.** `AddAssignment` — `POST /api/v2/assignments`, `Content-Type:
   application/json`. A success returns **201** with the request echoed back.

5. **Handle failures.**
   - `400 The assignment data is invalid` — the BMS content or `bmsVer` failed validation.
     Audatex does not support every field in the CIECA BMS; check your mapping against the
     Audatex Estimate Return Data Mapping before assuming the BMS is at fault.
   - `401 The authentication token is either missing or invalid` — mint a new token.
   - There is **no** error body schema and no problem+json. You get a status code and a
     prose description; log the full response.

6. **Acknowledge.** Audatex posts an assignment acknowledgement to the `Assignment` route.
   Use `AssignmentAcks` (`POST /api/v2/assignmentAcks`) to see the expected shape.

## Critical constraints

- **There is no idempotency key.** POSTing the same assignment twice creates two
  assignments as far as the public contract is concerned. Deduplicate on your side, keyed
  on your own claim number / BMS RQUID, before you call.
- Your callback receiver **must** be idempotent: Audatex retries delivery up to 3 times
  (15 second timeout) and then drops the message into a reject queue, so you can and will
  see duplicates.
- Correlate with `Header.Rquid` (your BMS `AssignmentRq` RQUID) and `Header.Cid`. These are
  correlation ids, not deduplication keys.
- Callbacks carry **no signature**. Authenticate the caller with the credentials you issued
  in `responseRoute.authorization` and allowlist the Audatex egress IPs.

## Next

- `solera-retrieve-estimate-documents.md` — consume the estimate-complete callback.
- `solera-retrieve-claim-images.md` — consume the claim-image callback.
