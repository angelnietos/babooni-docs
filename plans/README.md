# Planes

Planes de trabajo del monorepo. **No son la biblia operativa** — si contradicen
`docs/README.md` / `architecture/` / `guides/`, gana la biblia.

## Estructura

- **`rounds/`** — rondas `plans-{n}-{nombre}-round/` (histórico + activas).
- **`global/`** — políticas transversales (`F00-*`).

Estado por plan: `listo para ejecutar` | `en progreso` | `completado`.

## Rondas activas / pendientes

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-49-forty-nine-round](./rounds/plans-49-forty-nine-round/) | listo para ejecutar | Deep imports finales + any-cast + FE typecheck |
| [plans-47-forty-seven-round](./rounds/plans-47-forty-seven-round/) | en progreso | Cierre deuda F43 + typecheck + polish |

## Rondas completadas (archivo)

| Ronda | Tema |
|-------|------|
| [plans-48](./rounds/plans-48-forty-eight-round/) | Typecheck hygiene + barrels + deep-imports + any-cast |
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
