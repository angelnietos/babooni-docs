# F56-C1 — MockServer global (fixtures + CLI)

## Estado

completado

## Objetivo

MockServer HTTP workspace para FE sin Nest/Postgres/Keycloak.

## Criterios de aceptación

- [x] `pnpm mockserver` arranca; `/health` + ≥2 rutas `/api/*`.
- [x] Runbook con puertos y proxy FE.
- [x] Sin Docker/Postgres para el mock.

## Resultado

Stack: **Node + Express** en `tools/mockserver/`.
OIDC stub (`/realms/arquetipos` + password grant) + fixtures clients/users/audit/roles/auth/me.
`pnpm mockserver` (:4010), `pnpm mockserver:smoke`.
Runbook: [mockserver.md](../../../runbooks/mockserver.md).
