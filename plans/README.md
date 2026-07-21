# Planes

Planes de trabajo del monorepo. **No son la biblia operativa** — si contradicen
`docs/README.md` / `architecture/` / `guides/`, gana la biblia.

## Estructura

- **`rounds/`** — rondas `plans-{n}-{nombre}-round/` (histórico + activas).
- **`global/`** — políticas transversales (`F00-*`).

Estado por plan: `listo para ejecutar` | `en progreso` | `completado` | `trasladado`.

## Ronda activa

| Ronda | Estado | Tema |
|-------|--------|------|
| — | — | (ninguna; F56 cerrada) |

## Siguiente (preparada)

| Ronda | Estado | Tema |
|-------|--------|------|
| — | — | F57: Chromatic / Code Connect / remove `@deprecated` + fix TS6059 react features tsc |

## Rondas completadas (archivo)

| Ronda | Tema |
|-------|------|
| [plans-56](./rounds/plans-56-fifty-six-round/) | Apps arquetipos build/visual, Jest BE, MockServer FE-only |
| [plans-55](./rounds/plans-55-fifty-five-round/) | Carry F54: Lit 2, a11y, coverage BE strict, Jest workspace, parity strict |
| [plans-54](./rounds/plans-54-fifty-four-round/) | Migración native wrappers, oleadas Lit, tokens, SB CI, parity, gate strict |
| [plans-53](./rounds/plans-53-fifty-three-round/) | Native-ui SoT + freeze + wrappers/SB (cerrado en paralelo con F54) |
| [plans-52](./rounds/plans-52-fifty-two-round/) | Publish workflow + workspace-deps strict + storybook + coverage soft + peers mobile |
| [plans-51](./rounds/plans-51-fifty-one-round/) | Workspace Ionic + cobertura BE + mobile layers + SaaS typecheck + npm publish canario |
| [plans-50](./rounds/plans-50-fifty-round/) | Typecheck FE + tests BE + int-specs + Metro/CI |
| [plans-49](./rounds/plans-49-forty-nine-round/) | Deep-imports + any impl. (A3/A4 → F50) |
| [plans-48](./rounds/plans-48-forty-eight-round/) | Typecheck hygiene + barrels + deep-imports + any-cast specs |
| [plans-47](./rounds/plans-47-forty-seven-round/) | Superseded por F48/F49 (harness / deep-imports / josanz-ui) |
| [plans-46](./rounds/plans-46-forty-six-round/) | Typecheck hygiene + DX |
| [plans-45](./rounds/plans-45-forty-five-round/) | Cierre F44 + typecheck + Node/Nx + harness |
| [plans-44](./rounds/plans-44-forty-four-round/) | Deprecation hygiene + versionado |
| [plans-43](./rounds/plans-43-forty-three-round/) | Facade consolidation + cleanup F42 |
| [plans-42](./rounds/plans-42-forty-two-round/) | Harness + deep imports (parcial → F43) |
| [plans-41](./rounds/plans-41-forty-one-round/) | Deuda principal / convenciones |

## Planes globales

- [global/F00-node-nx-compatibility.md](./global/F00-node-nx-compatibility.md) — Node.js ↔ Nx

## Enlaces

- [docs/README.md](../README.md) — biblia
- [CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md) — estilo de docs
