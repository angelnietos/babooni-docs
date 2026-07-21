# Ronda F50 — Motor de empresa: typecheck FE + tests + calidad + DX docs

## Estado

completado

## Dependencias externas

- F49 cerrada (parcial): A1/A2/B1 hechos; A3/A4 → esta ronda (cerrados).
- Biblia operativa ampliada (docs motor).
- F47 cerrada como superseded (F50-B1).

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F50-A1 | [Frontend typecheck coverage](1750000050000-f50-frontend-typecheck-coverage.md) | completado |
| F50-A2 | [Tests backend faltantes](1750000051000-f50-add-missing-backend-tests.md) | completado |
| F50-A3 | [Tipar int-specs / residual `as any`](1750000052000-f50-cleanup-any-casts-in-int-specs.md) | completado |
| F50-B1 | [Cerrar rondas zombie F47](1750000053000-f50-close-stale-rounds.md) | completado |
| F50-C1 | [DX mobile Metro + matriz puertos](1750000054000-f50-mobile-metro-and-ports-dx.md) | completado |
| F50-C2 | [Gates CI ↔ biblia (PR checklist)](1750000055000-f50-align-ci-gates-with-bible.md) | completado |
| F50-D1 | [Documentación y cierre F50](1750000056000-f50-documentation.md) | completado |

## Objetivo (cumplido)

1. Carry F49: typecheck FE + tests backend + tipado int-specs.
2. Higiene planes: F47 superseded.
3. DX Metro React-18 pin en `tools/metro/` + puertos documentados.
4. CI ↔ docs: `pr-checklist` alineado.
5. Docs / índices cerrados → F51.

## Carry-over → F51

- Cobertura formal ≥ 80% + reporte CI (audit/settings/tenants).
- Phantom workspace deps Ionic (bloquean `pnpm install` en algunos filtros).
- Capas incompletas dashboard mobile (lib-layout warn).
- SaaS CRM libs typecheck más allá de la app canario.

Ver [plans-51-fifty-one-round](../plans-51-fifty-one-round/README.md).

## Criterios de aceptación

- [x] Typecheck FE objetivo verde (incl. verifactu-platform spec bundler).
- [x] ≥ 10 tests nuevos; suite `base-backend` verde.
- [x] Int-specs domains sin `as any`.
- [x] F47 superseded.
- [x] Metro shared en `tools/` + lint boundaries OK.
- [x] Hub + plans README: F50 archivo; F51 activa.
- [x] `pnpm check:deprecated` + `pnpm check:exports-paths` pasan.
