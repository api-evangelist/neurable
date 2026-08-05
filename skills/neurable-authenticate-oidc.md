---
name: neurable-authenticate-oidc
description: >-
  Obtain an access token from the Neurable identity provider at pipe.neurable.com using the OpenID
  Connect authorization-code flow with mandatory PKCE, or client credentials. Use before calling any
  Neurable operation tagged protected. Note the non-standard required `audience` parameter and that
  Neurable does not publish which scope covers which operation.
api: Neurable Pipe Service
base_url: https://pipe.neurable.com
spec: openapi/neurable-pipe-service-openapi.yml
operations:
  - get_oauth_authorize_oauth_authorize_get
  - post_oauth_token_oauth_token_post
  - get_well_known_openid_configuration__well_known_openid_configuration_get
  - get_well_known_jwks__well_known_jwks_json_get
  - get_userinfo_oidc_userinfo_get
  - get_me_me_get
generated: '2026-08-04'
method: generated
source: >-
  openapi/_original/neurable-pipe-openapi.json +
  https://pipe.neurable.com/.well-known/openid-configuration (probed 2026-08-04, HTTP 200)
---

# Authenticate against Neurable

## What exists

Neurable runs a real OpenID Connect provider at `https://pipe.neurable.com`, co-located with its
real-time EEG processing service. It publishes discovery metadata and a JWKS. It publishes **no**
prose documentation about any of it — everything below is read off the discovery document and the
service's own OpenAPI.

Start from discovery, not from this file:

`GET https://pipe.neurable.com/.well-known/openid-configuration`

## Authorization code + PKCE (mandatory)

`GET /oauth/authorize` (`get_oauth_authorize_oauth_authorize_get`)

Required query parameters — note there are **seven**, not the usual four:

| Parameter | Notes |
|---|---|
| `client_id` | |
| `redirect_uri` | |
| `response_type` | only `code` is supported |
| `code_challenge` | **required** — PKCE is not optional here |
| `code_challenge_method` | **required** — only `S256` is supported |
| `scope` | see below |
| `audience` | **required and non-standard.** Not part of OIDC Core; resource-targeting in the spirit of RFC 8707, but Neurable uses `audience` rather than the RFC's `resource`, and does not document the accepted values. |

Optional: `state`, `nonce`. Use `state` regardless.

`code_challenge` and `code_challenge_method` being *required* is the single most important
difference from a stock OIDC client — a library that treats PKCE as opt-in will fail here.

## Token exchange

`POST /oauth/token` (`post_oauth_token_oauth_token_post`)

Body is **`application/x-www-form-urlencoded`** (schema
`Body_post_oauth_token_oauth_token_post`), not JSON. Response is `PostOAuthTokenResponse`.

Grants advertised in discovery: `authorization_code`, `refresh_token`, `client_credentials`.

Client authentication: `token_endpoint_auth_methods_supported` is `["none", "client_secret_post"]`.
`none` means **public clients are supported** — appropriate for the mobile app, and the reason PKCE
is enforced. Confidential clients post `client_secret` in the body; there is no HTTP Basic option.

## Scopes

Advertised in discovery: `openid`, `email`, `demos:all:read`, `demos:prime:read`,
`session:stream:create`.

`scope` is a **required** parameter on `/oauth/authorize`, so you must pick. But **no Neurable
OpenAPI document maps any operation to any scope** — none of the three declares a `securitySchemes`
block at all. Which scope unlocks which `protected` operation is not published. Request the
narrowest scope that plausibly matches your use and confirm with Neurable; see
`scopes/neurable-scopes.yml` for what each one appears to govern.

## Validating tokens

`GET /.well-known/jwks.json` (`get_well_known_jwks__well_known_jwks_json_get`) returns one RSA
signing key (`alg` RS256, `use` sig, `kid`
`hXAk-wC5HGI9XBU6M3oLZlysD9iuxsSrS3GSCiQmX88` as of 2026-08-04).

Validate ID tokens against:

- `iss` = `https://pipe.neurable.com` (exact)
- signature algorithm `RS256`, key resolved by `kid` from the JWKS
- `aud`, `exp`, `iat`, and `nonce` when you sent one

Claims available: `iss`, `sub`, `aud`, `exp`, `iat`, `nonce`, `email`. `claims_parameter_supported`
is `false` — you cannot request specific claims via the `claims` parameter.

Cache the JWKS and key it by `kid`. There is a single key today, so plan for a rotation you will
not be told about: no changelog, deprecation policy or status page exists.

## Reading the principal

- `GET /oidc/userinfo` (`get_userinfo_oidc_userinfo_get`) → `GetUserInfoResponse`
- `GET /me` (`get_me_me_get`) → `GetMeResponse`

Both are declared without a `security` requirement in the served spec, which is a documentation
defect rather than a statement that they are anonymous.

## Health check

`GET /version` (`get_version_version_get`) → `{"version": "0.0.24"}`. The only version signal
Neurable offers on any service; the other two report an `arm64-<git-sha>` build string instead.

## Related

- `authentication/neurable-authentication.yml`
- `scopes/neurable-scopes.yml`
- `conformance/neurable-conformance.yml` — what Neurable does and does not conform to
- `well-known/neurable-well-known.yml`
