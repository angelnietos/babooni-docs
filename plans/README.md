<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Planes</h1>

<p align="center">
  Trabajo en curso · <b>no son la biblia operativa</b>
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./rounds/plans-85-eighty-five-round/"><img alt="F85" src="https://img.shields.io/badge/F85-active-14b8a6?style=flat-square" /></a>
</p>

Planes de trabajo del monorepo. **No son la biblia operativa** — si contradicen
`docs/README.md` / `architecture/` / `guides/`, gana la biblia.

> Rondas cerradas (F41–F84, incl. F78/F83) se eliminaron del árbol. Recuperables en el
> historial de git. Paths `tools/scripts/…` en planes antiguos → [legacy-paths.md](../legacy-paths.md)
> y [tools-layout.md](../runbooks/tools-layout.md).

## Estructura

- **`rounds/`** — solo rondas **activas** o listas para ejecutar.
- **`global/`** — políticas transversales (`F00-*`).

Estado por plan: `listo para ejecutar` | `en progreso` | `completado` | `trasladado`.

## Rondas en el árbol

| Ronda | Estado | Tema |
|-------|--------|------|
| [plans-85](./rounds/plans-85-eighty-five-round/) | activa | **Motor monorepo / base**: migrate multi-provider real, SaaS adapter alignment, portability hardening |

## Fuera de `docs/plans/` (producto)

| Tema | Dónde |
|------|-------|
| Ideauto Recalls — ejecución M0–M6 | **[docs/ideauto/migration/](../ideauto/migration/)** |
| Ideauto Recalls — narrativa | [docs/ideauto/recalls/](../ideauto/recalls/) |
| DB portability (cerrado F83) | [database-providers.md](../runbooks/database-providers.md) · [ADR 0014](../adr/adr-0014-database-provider-portability.md) |

## Planes globales

- [global/F00-node-nx-compatibility.md](./global/F00-node-nx-compatibility.md) — Node.js ↔ Nx

## Enlaces

- [docs/README.md](../README.md) — biblia
- [CONTRIBUTING-DOCS.md](../CONTRIBUTING-DOCS.md) — estilo de docs
