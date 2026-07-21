# F56-B1 — Extender Jest / coverage al backend

## Estado

listo para ejecutar

## Objetivo

F55-C3 unificó el **preset** Jest (incl. backends con `jest.preset.js`). F56
profundiza **cobertura y scripts** en backends Nest (apps arquetipos, josanz,
SaaS) sin tocar a la baja el gate estricto `test:cov:check` (audit/settings/tenants).

## Prerrequisitos

- `pnpm check:jest-preset` verde.
- `pnpm test:cov:check` strict en CI (F55-C1) intacto.

## Tareas

1. Inventariar backends con `test` target: `api`, `api-single`, `api-gateway`,
   `clients-ms`, `josanz-api` / `@josanz/backend`, `@saas/*-backend`.
2. Asegurar `coverageDirectory` vía shared + reporters `json` (merge global).
3. Oleada 1: smoke/`passWithNoTests` en apps thin; tests reales en libs
   (`@base/backend`, `@josanz/backend`, saas).
4. Script opcional `pnpm test:coverage:backend` =
   `nx run-many -t test -p tag:runtime:backend -- --coverage` + merge.
5. CI soft: artifact coverage BE apps (aparte del gate dominios).
6. Documentar en [jest-coverage.md](../../../runbooks/jest-coverage.md) +
   testing-pyramid: apps Nest ≠ umbral F55-C1.
7. `check:jest-preset --strict` candidato cuando 0 missing.

## Criterios de aceptación

- [ ] Script/documentación coverage BE workspace (sin bajar umbrales C1).
- [ ] ≥1 backend app/lib adicional con coverage en `coverage/<root>/` mergeable.
- [ ] `test:cov:check` sigue fail-on-threshold.
