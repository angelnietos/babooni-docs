<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F56-C1 — MockServer global (fixtures + CLI)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

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
