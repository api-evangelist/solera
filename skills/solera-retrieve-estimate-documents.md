---
name: Retrieve estimate documents after an estimate completes
description: Receive the Audatex.Event.EstimateComplete HATEOAS callback and pull every referenced estimate XML, print image, attachment and valuation from the Audatex GetDocuments surface.
api: openapi/solera-getdocuments-v1-openapi.json
operations:
  - GET /api/v{version}/claims/{assignmentId}/document/{locator}
  - GET /api/v{version}/GetDocument
  - GET /api/v{version}/GetValuation
  - EstimateReturnResponse
generated: '2026-07-25'
method: generated
source: openapi/solera-getdocuments-v1-openapi.json, openapi/solera-getdocuments-v2-openapi.json, openapi/solera-enterprise-assignment-prod-swagger.json, Estimate Document Return API Integration Guide sections 6.3-6.4
---

# Retrieve estimate documents after an estimate completes

Audatex does not let you poll for a finished estimate. It **pushes** to you, then you pull
the documents it names. This skill covers both halves.

> Operation ids: the harvested GetDocuments specification omits `operationId` on every
> operation. `overlays/solera-getdocuments-v1-overlay.yaml` assigns
> `getClaimDocument`, `getDocumentById`, `getValuationById` and
> `getGetDocumentsApiVersion` so these can be referenced from workflows. The underlying
> METHOD + path shown below is what the provider actually publishes.

## Prerequisites

- A bearer token with scope `b2b.fnol.documents` (see `solera-authenticate.md`).
- A live callback endpoint registered at assignment time as `responseMessage[].type =
  "EstimateReturn"` (see `solera-dispatch-assignment.md`).

## Steps

1. **Receive the callback.** Audatex authenticates against your authorization server using
   the credentials you supplied, then POSTs a HATEOAS message to your `EstimateReturn`
   endpoint. Headers carry `Rquid` and `MessageType`; the body is:
   - `Header.Rquid` — your BMS `AssignmentRq` RQUID
   - `Header.Cid` — Audatex correlation id
   - `Header.MessageType` — `Audatex.Event.EstimateComplete`
   - `Body.ClaimNumber`
   - `Body.Links[]` — one entry per document, with `Rel`, `Method`, `Href`, `Type`, `Info`
   - `Body.PassThroughData` — free text you supplied on the assignment, echoed back

2. **Return success immediately.** Persist the message and acknowledge before you start
   fetching. If you do not return `success`, Audatex retries up to 3 times with a 15 second
   timeout and then routes the message to its reject queue — at which point you have lost
   the notification and must contact support. Optionally ask your account representative to
   enable email notifications on rejection.

3. **Dedupe.** Retries mean at-least-once delivery. Key on `Header.Rquid` + `Header.Cid`
   and drop repeats.

4. **Walk `Body.Links[]`.** `Rel` values published in the guides are `claimXml`,
   `printImages`, `attachments` (and `claimImage` on the claim-image variant). Do not
   hardcode the URL shape — use `Href` as given. It resolves to
   `GET /api/v{version}/claims/{assignmentId}/document/{locator}` on the GetDocuments
   surface (`https://api-prod.audatex.com/GetDocuments` in production,
   `https://api-demo.audatex.com/GetDocuments` in demo).

5. **GET each document** with your `b2b.fnol.documents` bearer token. The response is the
   standard envelope:
   - `header.cid`, `header.statusCode` (`"OK"` in the published sample)
   - `body.claimNumber`, `body.createdForProfileId`, `body.imageId`, `body.attachmentId`,
     `body.requestNbr`, `body.extension`, `body.payload`, `body.dataType`

6. **Decode.** `body.payload` is Base64. `body.dataType` tells you what it is —
   `XML.Base64` for the CIECA BMS estimate document, `pdf.Base64` for PDFs — and
   `body.extension` gives the file extension. Attachments carry `Info.Extension`,
   `Info.FileType` and `Info.AttachmentDatetime` on the link.

7. **Map the estimate.** The decoded XML is a CIECA BMS 5.7 message. "It will be the
   Client's responsibility to convert the data in the CIECA BMS format to the format
   required by their system." Audatex does **not** implement every BMS field — use the
   Audatex Estimate Return Data Mapping, and get the BMS 5.7 schema from cieca.com (CIECA
   membership may be required).

8. **Valuations.** Total-loss / market valuations come from the same surface:
   `GET /api/v{version}/GetValuation` (by id) on GetDocuments 1.0, or
   `GET /api/v{version}/valuation/{type}/{processId}` on GetDocuments 2.0 — published
   example `processId` `67302420-1`, types `xml` and `pdf`.

## Error handling

- `401` — token missing, expired, or carrying the wrong scope. `b2b.fnol.documents` is
  required here, not `b2b.fnol.api`.
- `400` — invalid parameter. The response description is the only detail you get; there is
  no error body schema and no RFC 9457 problem+json.
- No `404`, `429` or `5xx` is declared anywhere in the specification. Treat anything else
  as retryable with backoff, and remember there is no published rate limit but there IS a
  documented availability window.

## Related

- `errors/solera-problem-types.yml`, `conventions/solera-conventions.yml`
- `asyncapi/solera-eapi-asyncapi.yml` for the full event shape
