---
name: Authenticate against the Audatex IdentityServer
description: Obtain and use an OAuth 2.0 bearer token for any Solera / Audatex EAPI surface, choosing the right scope for the API you are about to call.
api: https://dispatch-login.audatex.com/.well-known/openid-configuration
operations: []
generated: '2026-07-25'
method: generated
source: well-known/solera-openid-configuration.json, authentication/solera-authentication.yml, Estimate Document Return API Integration Guide section 6.4.1
---

# Authenticate against the Audatex IdentityServer

Every Solera / Audatex EAPI call is bearer-token authenticated. There is no API key and no
self-serve credential issuance — the `client_id`, `client_secret`, Audatex user id and
password are provisioned by an Audatex account representative during onboarding. Do not
attempt to register; if you do not have credentials, the integration has not been set up.

## Before you start

- Confirm which environment you are in:
  - Production: `https://dispatch-login.audatex.com` / `https://api-prod.audatex.com`
  - Demo / UAT: `https://dispatch-login-demo.audatex.com` / `https://api-demo.audatex.com`
- Confirm the Audatex egress IP has been allowlisted on your side if you will receive
  callbacks: Tanzu Test `170.76.172.18`, Tanzu Prod `170.76.172.20`.

## Steps

1. **Pick the scope for the API you are about to call.** Requesting the wrong scope is the
   most common cause of `401 Authorization has been denied for this request`.
   - `b2b.fnol.api` — Assignment API, ClaimImages API, GIC Integration API, EAPI Get Document API
   - `b2b.fnol.documents` — Audatex GetDocuments API (document and valuation retrieval)
   - Additional resource scopes advertised by the IdentityServer include `gofnol.api`,
     `b2b.fnol.transformer`, `b2b.admin.api`, `estimatics.api`, `hqclaims.api`,
     `novo.claims.manager`, `novo.estimating`, `mobile.inspection.api`,
     `marketdata.web.api`, `vinhistory.api`, `vindecode.api`, `eagle.api`, `PartsTrader.api`.
   - You may request **more than one scope on a single token**, space separated, and reuse
     that token across APIs.

2. **POST the token request** as `application/x-www-form-urlencoded` to
   `<identity-host>/connect/token` with `grant_type=client_credentials`, your provisioned
   `client_id` and `client_secret`, the `scope`, and the Audatex `username` / `password`
   where your integration was configured for the resource-owner flow. The harvested
   specifications declare the `password` flow; the integration guides document
   `client_credentials`. Use whichever your account representative provisioned.

3. **Read the response.** It returns `access_token`, `token_type` (`Bearer`) and
   `expires_in` (43200 seconds — 12 hours — in the published sample). Cache the token until
   shortly before expiry rather than minting one per call.

4. **Send it** on every API call as `Authorization: Bearer <access_token>`.

5. **On `401`** — `The authentication token is either missing or invalid` or
   `Authorization has been denied for this request` — mint a fresh token and confirm the
   scope matches the target API. Do not retry the same token.

## Alternative

Both return-API integration guides state that HTTP **Basic Authorization** is supported as
an alternative to OAuth, over SSL. Use it only if your account was provisioned for it.

## Notes

- OIDC discovery is anonymous and current on both hosts; read it rather than hardcoding
  endpoints: `/.well-known/openid-configuration`.
- PKCE (`S256`), device authorization, token introspection and revocation endpoints are all
  advertised. `/.well-known/oauth-authorization-server` is **not** served — only the OIDC
  discovery path.
- There is no published rate limit. There IS a published availability window
  (06:30–00:00 EST Mon–Sat, 10:00–14:00 EST Sun); see `lifecycle/solera-lifecycle.yml`.
