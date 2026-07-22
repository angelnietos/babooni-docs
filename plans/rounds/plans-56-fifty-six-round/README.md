# Ronda F56 — Apps arquetipos verdes, Jest BE, MockServer FE-only

## Estado

completado

> Cerrada 2026-07-22. Carry → **[F57](../plans-57-fifty-seven-round/)**: Chromatic/Code Connect/remove deprecated;
> fix TS6059 `@nx/js:tsc` en `arquetipos-react-*-features` (rootDir); coverage truth; scaffolds.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F56-A1 | [Build smoke apps arquetipos](1750000130000-f56-arquetipos-apps-build-smoke.md) | **completado** |
| F56-A2 | [Visual / serve QA](1750000131000-f56-arquetipos-apps-visual-qa.md) | **completado** (checklist + mock) |
| F56-B1 | [Jest + coverage backend](1750000132000-f56-backend-jest-coverage-extend.md) | **completado** (script) |
| F56-C1 | [MockServer global](1750000133000-f56-mockserver-global.md) | **completado** |
| F56-C2 | [FE → proxy mock](1750000134000-f56-fe-mockserver-wiring.md) | **completado** |
| F56-D1 | [Carry Chromatic / Code Connect / deprecated](1750000135000-f56-carry-chromatic-code-connect-deprecated.md) | **defer F57** |
| F56-E1 | [Documentación](1750000136000-f56-documentation.md) | **completado** |

## Resultado (ronda)

1. `pnpm arq:fe:build:smoke` — Angular single production OK; React Vite con
   `--excludeTaskDependencies` (tsc libs features TS6059 → F57).
2. MockServer Node Express en `tools/mockserver/` (`pnpm mockserver` :4010) —
   OIDC stub + fixtures `/api/*`; `pnpm mockserver:smoke` verde.
3. FE mock: `arq:fe:angular-single:mock` / `arq:fe:react-single:mock`.
4. `pnpm test:coverage:backend`; `test:cov:check` intacto.
5. Docs: runbook mockserver, local-dev, parity, ci-gates, jest-coverage.
6. Chromatic / Code Connect / remove deprecated → F57.

## Criterios de aceptación (ronda)

- [x] ≥2 apps build smoke (comando + CI soft).
- [x] Visual soft vía checklist mock documentada.
- [x] Script coverage BE + docs.
- [x] MockServer + FE mock sin API.
- [x] Runbook + local-development.
- [x] Carry D1 defer F57.
- [x] Hub alineado.
