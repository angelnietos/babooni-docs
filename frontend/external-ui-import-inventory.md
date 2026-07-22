<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Inventario UI externo → Lit (F53-A3 / F54-A2 / F55-A1)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Snapshot 2026-07-22. No hay catálogo externo con licencia/credenciales en el
workspace; la priorización usa **gaps** del design system interno
(`@base/angular-ui` / `@base/react-ui` sin CE) y backlog de átomos.

## Top-10 (prioridad)

| # | Componente | Origen | Decisión |
|---|------------|--------|----------|
| 1 | Divider | gap interno | **Oleada 1** → `base-divider` |
| 2 | Progress | gap interno | **Oleada 1** → `base-progress` |
| 3 | Chip / tag removable | gap (badge ≠ chip) | **Oleada 1** → `base-chip` |
| 4 | Select | angular-ui `select` | **Oleada 2** → `base-select` |
| 5 | Icon set | angular/react icon | **Oleada 2** → `base-icon` |
| 6 | Pagination | numbered pagination | **Oleada 2** → `base-pagination` |
| 7 | Table | complejo | reject CE / keep framework |
| 8 | Filter bar | shared | **reject CE** — composición `NativeInput` + `NativeChip` (+ botones) |
| 9 | Confirm dialog | overlaps modal | reject (use `base-modal`) |
| 10 | Tenant switch | chrome | reject (app shell) |

## Oleada 1 (completada F54)

DoD: CE + register + wrappers Angular/React + Arq + stories catalog + ownership.

| CE | Wrappers | Stories |
|----|----------|---------|
| `base-divider` | Native* / ArqNative* | catalog.stories |
| `base-progress` | Native* / ArqNative* | catalog.stories |
| `base-chip` | Native* / ArqNative* | catalog.stories |

## Oleada 2 (completada F55-A1)

| CE | Wrappers | Stories |
|----|----------|---------|
| `base-select` | Native* / ArqNative* | catalog.stories |
| `base-icon` | Native* / ArqNative* | catalog.stories |
| `base-pagination` | Native* / ArqNative* | catalog.stories |

## Residual → F56

Vacío para átomos P0. Filter bar: **no CE** — patrón de composición
documentado arriba (F55-A2). Remove de primitivos `@deprecated` framework-only
cuando el gate `check:deprecated` lo permita.
