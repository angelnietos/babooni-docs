# ADR 0005: JWT (propio) vs Keycloak

- Status: accepted — **Keycloak chosen** (OIDC, resource-server / JWKS)
- Date: 2026-07-08 (revisited 2026-07-09)

## Context

The platform needs authentication plus SSO/MFA. Two options were considered:

- **Self-issued JWT** (`@nestjs/jwt`): simple, zero external dependencies, but the
  platform owns credential storage, hashing, token revocation and MFA — a large
  security surface to build and operate correctly.
- **Keycloak (OIDC)**: mature IdP providing SSO, MFA, brute-force protection,
  realm/client management and standards-based token issuance.

The decision was reversed from the initial "self-issued JWT for v1" to **Keycloak**
because SSO/MFA and enterprise IdP features are first-class requirements for the
product and would otherwise have to be rebuilt by hand.

## Decision

Use **Keycloak as the IdP**. The NestJS backend is a pure **OAuth2 resource
server**: it validates Keycloak-issued access tokens against the realm's JWKS
endpoint (`JwtStrategy` in `libs/base/backend/.../common/auth/jwt.strategy.ts`,
RS256, `iss`/`aud` checked) and never issues its own tokens. Login/SSO/MFA and
logout are delegated to Keycloak:

- `GET /api/auth/login` (and `/api/auth/logout`) redirect to Keycloak.
- The SPA performs the OIDC **Authorization Code + PKCE** flow directly against
  Keycloak (`@base/angular` `AuthService`), then sends the access token as a
  `Bearer` header; the backend verifies it via JWKS.

The `keycloak-connect` adapter is intentionally **not** used — for a stateless
API resource server, JWT/B bearer verification via JWKS is the correct pattern.
Secrets (DB, KMS, Kafka, `KEYCLOAK_CLIENT_SECRET`) are read from
`SecretsService`; the client secret is never embedded in the SPA (public client +
PKCE).

## Consequences

- Zero credential storage / hashing in the backend.
- Token revocation relies on Keycloak session/revocation lists + short TTL.
- The frontend owns the OIDC PKCE flow and callback handling.
- A `keycloak` service is part of `docker-compose.dev.yml`; realm/client must be
  provisioned (realm `arquetipos`, client `arquetipos-web`, PKCE enabled).

### Access-token TTL (dev)

`deploy/keycloak/realm-arquetipos.json` does **not** override realm token
lifespans, so Keycloak defaults apply (access token ≈ **5 minutes**). That is
intentional (short TTL + refresh). SPAs must persist `refresh_token` and call
`ensureAccessToken` / refresh on 401; both Angular and React kernels also show a
session-expiry warning (~90 s before `exp`) so idle users get a proper logout
instead of silent API failures.

To lengthen the access token in a local realm (optional): Keycloak Admin → realm
`arquetipos` → **Realm settings → Tokens → Access Token Lifespan**, or set
`accessTokenLifespan` in the realm import JSON and re-import.

## See also

- Token principal shape + guards: `libs/base/backend/src/lib/common/auth/jwt.strategy.ts`, `JwtAuthGuard`.
- Frontend OIDC PKCE flow: `@base/angular` `AuthService` (SPA does auth code + PKCE directly).
- Provisioning: `deploy/keycloak/realm-arquetipos.json` + `docker-compose.dev.yml` (`start-dev --import-realm`).
- In **e2e tests** the `JwtAuthGuard` is overridden by `TestAuthGuard` (decodes the bearer token without verifying the signature) — see `apps/clientes/josanz/backend/src/app/app.e2e-spec.ts`.
- Back to the [docs hub](../README.md).
