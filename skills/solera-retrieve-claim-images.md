---
name: Retrieve claim images
description: Consume the claim-image callback and fetch claim photos and decoded document files from the Audatex ClaimImages and GetImage surfaces.
api: openapi/solera-claim-images-prod-swagger.json
operations:
  - valuationReturn
  - decodeImage
  - GET /api/v{version}/claims/document/{locator}
generated: '2026-07-25'
method: generated
source: openapi/solera-claim-images-prod-swagger.json, openapi/solera-claim-images-openapi.json, openapi/solera-getimage-v1-openapi.json, Claim Image Document Return API Integration Guide
---

# Retrieve claim images

The claim-image flow mirrors the estimate-document flow: Audatex pushes a HATEOAS link,
you fetch the image. Two Audatex surfaces serve images — the **ClaimImages API**
(`/TestClaimImageapi`) and the **GetImage** surface (`/GetImage`), and the callback tells
you which one to use by giving you the full `Href`.

> Naming caveat, verbatim from the specification: the ClaimImages production Swagger names
> its image-by-id operation `valuationReturn` and its decoded-file operation `decodeImage`.
> Those `operationId`s are the provider's, not ours, and they do not match what the
> operations do. Use them as written when referencing the spec.

## Prerequisites

- A bearer token — scope `b2b.fnol.api` for the ClaimImages API, `b2b.fnol.documents` for
  the GetDocuments/GetImage surfaces (see `solera-authenticate.md`).
- A callback endpoint registered as `responseMessage[].type = "ClaimImageReturn"` at
  assignment time. If you did not register one, Audatex delivers claim images to your
  `EstimateReturn` endpoint instead — handle both `Rel` values on the same receiver.

## Steps

1. **Receive the callback.** `Header.MessageType` is `Audatex.Event.EstimateComplete` (the
   same discriminator as the estimate flow — branch on `Body.Links[].Rel`, not on
   `MessageType`). The claim-image link has `Rel: claimImage` and an `Href` pointing at
   `https://api-prod.audatex.com/GetImage/api/v1/claims/document/{locator}`.

2. **Return success immediately**, then dedupe on `Header.Rquid` + `Header.Cid`. Delivery
   is at-least-once with 3 retries.

3. **Fetch the image.** Follow `Href` as given —
   `GET /api/v{version}/claims/document/{locator}` on the GetImage surface. That surface
   declares an `apiKey`-style `Bearer` security scheme (`Authorization` header) rather than
   the `oauth2` scheme the other specs declare; the credential is the same bearer token,
   and several surfaces also require an explicit `api-version` — as a **header** on
   GetImage, as a **query parameter** (default `2.0`) on the ClaimImages production spec.

4. **Or fetch directly by ClaimImage id.** `valuationReturn` —
   `GET /api/v{version}/image/{ClaimImage}` on the ClaimImages API. Published example
   identifier: `C293B120-A763-404E-AC9D-2A687128D3DC`. A success returns **201**, not 200.

5. **Decoded files.** `decodeImage` — `GET /api/v{version}/image/{type}` — returns a decoded
   file described by the `FileResult` schema (`contentType`, `fileDownloadName`,
   `lastModified`, `entityTag`, `enableRangeProcessing`). On the demo OpenAPI 3.0.1 the same
   operation is exposed as `GET /api/v2/decodedFile`.

6. **Decode the payload.** As with documents, the image comes back Base64 in
   `body.payload`, with `body.imageId`, `body.extension` (e.g. `pdf`) and `body.dataType`
   (e.g. `pdf.Base64`). Nothing is streamed as binary.

## Error handling

- `400 The ClaimImage is invalid` — the identifier is not a resolvable claim image GUID.
- `400 The type is invalid` — the requested type is not one the endpoint decodes.
- `401 The authentication token is either missing or invalid` — re-mint the token, and
  check you are using the right scope for the surface you are calling.

## Related

- `sandbox/solera-sandbox.yml` for the published example identifiers.
- `data-model/solera-data-model.yml` for how ClaimImage relates to Claim and Document.
