# F49-A1 — Eliminar imports profundos restantes en base-backend

## Estado

completado

## Resultado

Cero imports `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`
(excl. specs). Los `handlers/index.ts`, `queries/index.ts` y `audit-context.ts`
usan barrels `@base/backend`.

Trabajo ejecutado en la rama (post-F48 / cierre F49):
- Batches de handlers/queries migrados a `REPOSITORY_TOKENS`,
  `makeCrudCommandHandlers`, `makeCrudQueryHandlers`.
- `audit-context` importa `AuthedRequest` desde `@base/backend`.

## Objetivo (histórico)

Completar la limpieza de imports profundos iniciada en F47-A3 y F48-A3.

## Criterios de aceptación

- [x] Cero `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts` (excluyendo `.spec.ts`).
- [x] Imports vía barrel `@base/backend`.
