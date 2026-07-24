<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Planes</h1>

<p align="center">
  Trabajo en curso · <b>no son la biblia operativa</b>
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./rounds/plans-84-eighty-four-round/"><img alt="F84" src="https://img.shields.io/badge/F84-active-14b8a6?style=flat-square" /></a>
  <a href="./rounds/plans-83-eighty-three-round/"><img alt="F83" src="https://img.shields.io/badge/F83-active-14b8a6?style=flat-square" /></a>
  <a href="./rounds/plans-78-seventy-eight-round/"><img alt="F78" src="https://img.shields.io/badge/F78-ready-0d5f59?style=flat-square" /></a>
</p>

Planes de trabajo del monorepo. **No son la biblia operativa** — si contradicen
`docs/README.md` / `architecture/` / `guides/`, gana la biblia.

> Rondas cerradas (F41–F82 excepto F78) se eliminaron del árbol. Recuperables en el
> historial de git. Paths `tools/scripts/…` en planes antiguos → [legacy-paths.md](../legacy-paths.md)
> y [tools-layout.md](../runbooks/tools-layout.md).

## Estructura

- **`rounds/`** — solo rondas **activas** o listas para ejecutar.
- **`global/`** — políticas transversales (`F00-*`).

Estado por plan: `listo para ejecutar` | `en progreso` | `completado` | `trasladado`.

## Rondas en el árbol

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-84](./rounds/plans-84-eighty-four-round/) | activa | **Ideauto Recalls_v2 → monorepo**: `clientes/ideauto/recalls` (`@ideauto/*`), strangler, MSSQL (F83) — **no SaaS** |
| [plans-83](./rounds/plans-83-eighty-three-round/) | activa | Database provider portability + adapters (Pg ⇄ SqlServer ⇄ MySql) |
| [plans-78](./rounds/plans-78-seventy-eight-round/) | listo para ejecutar | Feature parity React, mobile logic cleanup & architecture compliance |

Docs canónicos Recalls: **[docs/ideauto/recalls/](../ideauto/recalls/)** · [assessment](../architecture/recalls-v2-assessment.md) · [runbook](../runbooks/recalls-migration.md) · [ADR 0013](../adr/adr-0013-recalls-strangler-migration.md).

## Planes globales

- [global/F00-node-nx-compatibility.md](./global/F00-node-nx-compatibility.md) — Node.js ↔ Nx

## Enlaces

- [docs/README.md](../README.md) — biblia
- [CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md) — estilo de docs
