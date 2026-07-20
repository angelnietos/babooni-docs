# Auth / JWKS fail-closed (F38-H12)

## Contract

The API is a **Keycloak resource server**. Access tokens are verified with the
realm JWKS (`RS256`). There is **no** fallback to a shared HMAC secret, a
database session table, or an infinite stale JWKS cache.

| Situation | Behaviour |
|-----------|-----------|
| Valid token + known `kid` (cache hit or IdP up) | 200 / principal attached |
| Invalid / expired / missing `sub` | **401** |
| Unknown `kid` | **401** |
| JWKS HTTP timeout / IdP down (cache miss or expired) | **401** (fail closed) |

## Tunables

| Env | Default | Meaning |
|-----|---------|---------|
| `JWKS_TIMEOUT_MS` | `5000` | HTTP timeout for JWKS fetch |
| `JWKS_CACHE_MAX_AGE_MS` | `600000` (10m) | In-memory JWKS cache TTL |

Cache reduces Keycloak load; it is **not** offline auth. After TTL, the next
request must reach the IdP or it fails closed.

## Frontend

`@base/shared` `scheduleSessionExpiry` shows a **warning** window
(`SESSION_WARN_BEFORE_MS`, 90s) before access-token expiry, then `expired`.
Hosts (`SessionExpiryHost` / `lib-session-expiry`) must surface UI — never
clear the session without feedback.

## Specs

```bash
pnpm exec jest -c libs/base/backend/jest.config.ts --rootDir libs/base/backend \
  src/lib/platform/kernel/common/auth/jwks-options.spec.ts \
  src/lib/platform/kernel/common/auth/jwt.strategy.spec.ts \
  src/lib/platform/kernel/common/auth/session-expiry.spec.ts
```
