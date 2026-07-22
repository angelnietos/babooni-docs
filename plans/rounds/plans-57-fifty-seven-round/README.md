# Ronda F57 — Cobertura real, scaffolds unificados y carry F56

## Estado

completado

> Cerrada 2026-07-22. Carry → **F58**: Chromatic / Code Connect / remove `@deprecated`
> (sin token Figma/Chromatic). Coverage truth soft en CI; strict cuando inventario base sea estable.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F57-A1 | [Coverage report truth (por proyecto)](1750000140000-f57-coverage-report-truth.md) | **completado** |
| F57-A2 | [Coverage inventory + CI soft→strict](1750000141000-f57-coverage-inventory-ci.md) | **completado** (soft; strict → F58) |
| F57-B1 | [Scaffolds unificados (CLI + README)](1750000142000-f57-scaffolds-unified-cli.md) | **completado** |
| F57-B2 | [Scaffolds: tags runtime + SaaS/React parity](1750000143000-f57-scaffolds-runtime-saas-react.md) | **completado** |
| F57-C1 | [Fix TS6059 arquetipos-react-*-features](1750000144000-f57-react-features-rootdir-tsc.md) | **completado** |
| F57-D1 | [Carry: Chromatic / Code Connect / deprecated](1750000145000-f57-chromatic-code-connect-deprecated.md) | **defer F58** |
| F57-E1 | [Tools DX: checks scaffolds + smoke scaffolds](1750000146000-f57-tools-dx-scaffold-smoke.md) | **completado** |
| F57-F1 | [Documentación ronda](1750000147000-f57-documentation.md) | **completado** |

## Resultado (ronda)

1. `check-coverage-truth` + `report-coverage-inventory` + merge `projects.json`; CI soft tras coverage merge.
2. CLI `pnpm scaffold*` + `pnpm scaffolds:smoke`; tags nuevos sin `scope:*`.
3. Eliminado `@nx/js:tsc` build en libs arquetipos React features/shell/ui → `arq:fe:build:smoke` sin `--excludeTaskDependencies`.
4. Chromatic / Code Connect / remove deprecated → F58.

## Criterios de aceptación (ronda)

- [x] Informe coverage por proyecto + merge auditable.
- [x] Soft gate coverage-truth en CI.
- [x] CLI scaffolds + README + smoke.
- [x] `pnpm arq:fe:build:smoke` sin exclude React.
- [x] D1 defer F58.
- [x] Hub docs alineados.
