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
| [plans-53-fifty-three-round](./rounds/plans-53-fifty-three-round/) | listo para ejecutar | Native-ui SoT + freeze framework UI + Storybook serve/native + imports + gates |

## Siguiente (preparada)

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-54-fifty-four-round](./rounds/plans-54-fifty-four-round/) | listo para ejecutar | Migración features → native, oleadas import, deprecation, CI SB/visual, tokens, a11y, **paridad arquetipos**, gate strict |

> F54 arranca tras **F53-F1** (o en paralelo solo docs). No marcar F53
> `completado` desde la carpeta F54.

## Rondas completadas (archivo)

| Ronda | Tema |
|-------|------|
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
