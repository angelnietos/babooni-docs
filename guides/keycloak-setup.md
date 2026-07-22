<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Keycloak — setup local y producto</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: configurar realms/clients OIDC o depurar 401/JWKS.

Decisión: [ADR 0005](../adr/adr-0005-jwt-vs-keycloak.md) — Keycloak es el IdP;
el backend es **resource server** (valida JWKS, **no emite JWT**).

## Realms y clients

| Producto / plantilla | Realm | Client id típico |
|----------------------|-------|------------------|
| Josanz | `josanz` | `josanz-web` |
| Arquetipos | `arquetipos` | `arquetipos-web` |

Keycloak local: puerto **9080** (`pnpm dev:infra`). URL: `KEYCLOAK_URL`.

El bootstrap de `josanz-api` fuerza realm `josanz`. Si el SPA usa otro realm → 401.

## Backend

- Valida access tokens vía JWKS del realm (RS256, `iss` / `aud`).
- Fail-closed si JWKS no está disponible: [auth-jwks-fail-closed.md](./auth-jwks-fail-closed.md).
- Rutas públicas: `/api/auth/**` (demo), `/api/health/**` — el resto Bearer.

## Frontend

- SPA / Next / mobile: cliente OIDC público (PKCE) hacia el client id del producto.
- No hardcodear secrets de client confidential en el browser.
- Tras login, propagar Bearer en `@base/angular-api` / `@base/react-api`.

## Demo vs producción

| Modo | Auth |
|------|------|
| Demo mobile / algunas plantillas | Login local (`demoLogin`) sin Keycloak |
| Josanz / plantillas web con API | Keycloak obligatorio en entorno real |

`SERVICES.md` documenta `/api/auth` como demo + Keycloak en producción.

## Verificación

```bash
curl -i http://localhost:9080/realms/josanz
# Backend ready
curl -i http://localhost:3000/api/health/ready
```

401 con token válido → revisar `KEYCLOAK_REALM`, `KEYCLOAK_URL`, audience del client.

## Enlaces

- [local-development.md](./local-development.md)
- [roles-rbac.md](./roles-rbac.md)
- [SERVICES.md](../../SERVICES.md)
