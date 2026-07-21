# F56-B1 — Extender Jest / coverage al backend

## Estado

completado

## Objetivo

Profundizar coverage/scripts en backends Nest sin bajar `test:cov:check`.

## Criterios de aceptación

- [x] Script/documentación coverage BE workspace.
- [x] Coverage mergeable vía shared reporters.
- [x] `test:cov:check` sigue fail-on-threshold.

## Resultado

`pnpm test:coverage:backend` →
`nx run-many -t test -p tag:runtime:backend -- --coverage`.
Documentado en [jest-coverage.md](../../../runbooks/jest-coverage.md).
