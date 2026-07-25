---
name: Run the GIC image-capture integration
description: Post GIC integration payloads against a work assignment and acknowledge the M31 mobile image-upload-completed event.
api: openapi/solera-gic-integration-openapi.json
operations:
  - POST /api/v1/GICIntegration/{workAssignmentID}
  - POST /api/v1/getM31EventAcks
  - GET /api/Version
generated: '2026-07-25'
method: generated
source: openapi/solera-gic-integration-openapi.json, GIC - Image Capture API Integration Guide (DOCX)
---

# Run the GIC image-capture integration

The Global Integration Component (GIC) moves photos captured on a mobile client into an
Audatex Estimate (ADXE), together with VIN, mileage and user comments. Audatex EAPI is the
orchestrator: it pulls images from Audatarget, pushes them into Eagle, and ADXE then raises
an **M31** event when the upload completes.

> Operation ids: the harvested GIC specification omits `operationId` on every operation.
> `overlays/solera-gic-integration-overlay.yaml` assigns `postGicIntegration`,
> `postM31EventAcks` and `getGicApiVersion`.

## Prerequisites

- A bearer token with scope `b2b.fnol.api` for the GIC surface (see
  `solera-authenticate.md`). The wider GIC pipeline additionally uses `eagle.api` against
  Audatex/Eagle and `images.admin apigateway.imageattachment` / `imagerequests.admin`
  against Audatarget (`https://auth.auda-target.com/connect/token`) — those legs are
  operated by Audatex EAPI, not by you, unless your integration explicitly covers them.
- A provisioned work assignment. Published example ids: QA `290240018`, DEMO `332614611`.

## Steps

1. **Post the GIC integration payload.**
   `POST /api/v1/GICIntegration/{workAssignmentID}` with an optional `imageContent` query
   parameter. Returns 200 on success. The specification declares no request body schema —
   the shape comes from the integration guide.

2. **Create the image attachment message** once the image request is complete, so it lands
   on the service bus. The guide's published shape is:
   ```json
   {
     "imageCaptureId": "A215EB8D-A9E7-4835-B68C-9F345DC687B0",
     "estimaticImageAttachment": {
       "sourceSystemKey": "taskid",
       "sourceSystem": "ADXE",
       "country": "CA"
     }
   }
   ```

3. **Expect the M31 event.** After the last image uploads, Audatex sends the M31 (mobile)
   event to your endpoint:
   - `messageType` / `routingKey` — `adxe.estimate.image-upload-for-mobile-completed`
   - `id` — event GUID
   - `body.workAssignmentId`, `body.createdForProfileId`, `body.eventSource`
     (`adxe.estimate`), `body.eventSourceDetails`, `body.eventCode` (`M31`),
     `body.eventDescription`, `body.eventTime`, `body.roleCode`, `body.estimateStatus`,
     `body.claimGuid`, `body.claimNo`, `body.claimId`

4. **Acknowledge it.** `POST /api/v1/getM31EventAcks`. If Audatex does not get `success`
   back it retries **3 times every 10 seconds** and then routes the message to the reject
   queue.

5. **Check the deployed version** when debugging environment drift:
   `GET /api/Version?api-version=<version>`.

## Constraints

- No idempotency key. Dedupe M31 events on `id` + `body.workAssignmentId`.
- No signature on the inbound event; authenticate Audatex with the credentials you issued
  and allowlist the egress IPs (Tanzu Test `170.76.172.18`, Tanzu Prod `170.76.172.20`).
- The GIC specification declares only `200 Success` — no error responses are documented at
  all for this surface.

## Related

- `asyncapi/solera-eapi-asyncapi.yml` — the M31 channel and message schema.
- `conventions/solera-conventions.yml` — retry and delivery semantics.
