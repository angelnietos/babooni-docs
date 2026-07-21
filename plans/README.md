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
| [plans-51-fifty-one-round](./rounds/plans-51-fifty-one-round/) | activa / en progreso | Workspace Ionic + cobertura BE + mobile layers + SaaS typecheck + **npm publish/versionado** |

## Siguiente (preparada)

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-52-fifty-two-round](./rounds/plans-52-fifty-two-round/) | listo para ejecutar | Carry publish + workspace-deps strict + storybook layout + coverage CI soft + peers mobile + docs |

> F52 arranca tras **F51-D1** (o en paralelo solo docs C2). No marcar F51
> `completado` hasta que sus hitos pendientes (A3/B1/C*/E1/D1) cierren.

## Rondas completadas (archivo)

| Ronda | Tema |
|-------|------|
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
