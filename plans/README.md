<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Planes</h1>

<p align="center">
  Trabajo en curso · <b>no son la biblia operativa</b>
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./rounds/plans-72-seventy-two-round/"><img alt="F72" src="https://img.shields.io/badge/F72-active-14b8a6?style=flat-square" /></a>
</p>

Planes de trabajo del monorepo. **No son la biblia operativa** — si contradicen
`docs/README.md` / `architecture/` / `guides/`, gana la biblia.

> Paths `tools/scripts/…` en planes antiguos → ver [legacy-paths.md](../legacy-paths.md#tools-post-f56)
> y [tools-layout.md](../runbooks/tools-layout.md).

## Estructura

- **`rounds/`** — rondas `plans-{n}-{nombre}-round/` (histórico + activas).
- **`global/`** — políticas transversales (`F00-*`).

Estado por plan: `listo para ejecutar` | `en progreso` | `completado` | `trasladado`.

## Ronda activa

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-72](./rounds/plans-72-seventy-two-round/) | listo para ejecutar | cierre carries F71; paridad scaffolding features multi-framework |

## Rondas completadas (archivo)

| Ronda | Tema |
|-------|------|
| [plans-71](./rounds/plans-71-seventy-one-round/) | Carries F70 deferidos a F72; docs F71 |
| [plans-70](./rounds/plans-70-seventy-round/) | Board piloto tasks; Ionic audit rollout; Chromatic/Zod → F71 |
| [plans-67](./rounds/plans-67-sixty-seven-round/) | FeatureShell adoption; entity-view roles/users; store ratchet expand; Chromatic/Zod → F68 |
| [plans-66](./rounds/plans-66-sixty-six-round/) | Features SoC; FeatureShell cards/table; facade multi-dominio; entity piloto; Chromatic/Zod → F67 |
| [plans-65](./rounds/plans-65-sixty-five-round/) | Confirm+toast multi-stack; facade SoC clients (D1); ADR 0012; Chromatic/deprecated → F66 |
| [plans-64](./rounds/plans-64-sixty-four-round/) | Portal verify, Next listbox, mobile e2e, tokens, Storybook; Chromatic/validación → F65; CI jest-preset strict |
| [plans-63](./rounds/plans-63-sixty-three-round/) | Overlay/mobile parcial; carries → F64 |
| [plans-62](./rounds/plans-62-sixty-two-round/) | Temas, select, shell, features native-first, RN tokens; carries → F63 |
| [plans-60](./rounds/plans-60-sixty-round/) | 7–1 ui-styles, login/clients nativos DOM; temas/select/features → F62 |
| [plans-59](./rounds/plans-59-fifty-nine-round/) | E2E Next·Ionic·RN (C1); validación isomórfica + visual tooling → F60 |
| [plans-58](./rounds/plans-58-fifty-eight-round/) | Coverage strict, e2e arquetipos all, purge `scope:*`, mock soft; Chromatic/Code Connect/deprecated → F59 |
| [plans-57](./rounds/plans-57-fifty-seven-round/) | Coverage truth, scaffolds CLI, TS6059 React, carry Chromatic→F58 |
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
