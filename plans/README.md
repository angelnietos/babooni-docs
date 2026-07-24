<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Planes</h1>

<p align="center">
  Trabajo en curso · <b>no son la biblia operativa</b>
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./rounds/plans-86-eighty-six-round/"><img alt="F86" src="https://img.shields.io/badge/F86-active-14b8a6?style=flat-square" /></a>
</p>

Planes de trabajo del monorepo. **No son la biblia operativa** — si contradicen
`docs/README.md` / `architecture/` / `guides/`, gana la biblia.

> Rondas cerradas (F41–F85, incl. F78/F83/F85) se eliminaron del árbol. Recuperables en el
> historial de git. Paths `tools/scripts/…` en planes antiguos → [legacy-paths.md](../legacy-paths.md)
> y [tools-layout.md](../runbooks/tools-layout.md).

## Estructura

- **`rounds/`** — solo rondas **activas** o listas para ejecutar.
- **`global/`** — políticas transversales (`F00-*`).

Estado por plan: `listo para ejecutar` | `en progreso` | `completado` | `trasladado`.

## Rondas en el árbol

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-86](./rounds/plans-86-eighty-six-round/) | activa | **Base oficial**: platform-as-product, config server FE/BE, SQL Server/Azure, bootstrap, quality bar |

## Fuera de `docs/plans/` (producto)

| Tema | Dónde |
|------|-------|
| Ideauto Recalls — ejecución M0–M6 | **[docs/ideauto/migration/](../ideauto/migration/)** |
| Ideauto Recalls — narrativa | [docs/ideauto/recalls/](../ideauto/recalls/) |
| DB portability (F83+F85) | [database-providers.md](../runbooks/database-providers.md) · [ADR 0014](../adr/adr-0014-database-provider-portability.md) |
| Platform-as-product | [platform-as-product.md](../architecture/platform-as-product.md) |

## Planes globales

- [global/F00-node-nx-compatibility.md](./global/F00-node-nx-compatibility.md) — Node.js ↔ Nx

## Enlaces

- [docs/README.md](../README.md) — biblia
- [CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md) — estilo de docs
