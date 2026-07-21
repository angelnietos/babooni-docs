# Ronda F52 — Cierre deuda F51 + gates workspace + peers + docs publish

## Estado

completado

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F52-A1 | [Carry npm publish](1750000070000-f52-carry-npm-publish-and-versioning.md) | completado |
| F52-A2 | [workspace-deps:strict](1750000071000-f52-fix-undeclared-workspace-deps.md) | completado |
| F52-B1 | [Storybook lib-layout](1750000072000-f52-storybook-lib-layout-runtime-tag.md) | completado |
| F52-B2 | [Coverage CI soft](1750000073000-f52-ci-soft-coverage-gate.md) | completado |
| F52-C1 | [Peers mobile/Ionic](1750000074000-f52-peer-deps-mobile-ionic-alignment.md) | completado |
| F52-C2 | [Guía npm + biblia](1750000075000-f52-docs-npm-guide-and-bible-polish.md) | completado |
| F52-D1 | [Cierre](1750000076000-f52-documentation.md) | completado |

## Criterios de aceptación

- [x] Canario publish residual = 0 + workflow manual.
- [x] `pnpm check:workspace-deps:strict` exit 0.
- [x] `check-lib-layout` sin warn storybook.
- [x] Coverage soft en CI + docs.
- [x] Guía npm-publish enlazada.
- [x] Hub/plans: F52 archivo.
- [x] `check:deprecated` + `check:exports-paths` pasan.

## Carry → siguiente

- Coverage BE **strict** (fail job) tras ventana soft estable → F53-D1 / F54-D2.
- Ampliar oleada `publishable` / registry → F54-C2.
- **Ronda abierta:** [F53](../plans-53-fifty-three-round/) (native-ui SoT + SB).
- **Siguiente preparada:** [F54](../plans-54-fifty-four-round/) (migración + imports + visual CI).
