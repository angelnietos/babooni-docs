# Inventario UI externo → Lit (F53-A3 / F54-A2)

Snapshot 2026-07-22. No hay catálogo externo con licencia/credenciales en el
workspace; la priorización usa **gaps** del design system interno
(`@base/angular-ui` / `@base/react-ui` sin CE) y backlog de átomos.

## Top-10 (prioridad)

| # | Componente | Origen | Decisión |
|---|------------|--------|----------|
| 1 | Divider | gap interno | **Oleada 1** → `base-divider` |
| 2 | Progress | gap interno | **Oleada 1** → `base-progress` |
| 3 | Chip / tag removable | gap (badge ≠ chip) | **Oleada 1** → `base-chip` |
| 4 | Select | angular-ui `select` | F55 |
| 5 | Icon set | angular/react icon | F55 (sprite / CE) |
| 6 | Pagination | shared pagination | F55 |
| 7 | Table | complejo | reject CE / keep framework |
| 8 | Filter bar | shared | F55 |
| 9 | Confirm dialog | overlaps modal | reject (use `base-modal`) |
| 10 | Tenant switch | chrome | reject (app shell) |

## Oleada 1 (completada F54)

DoD: CE + register + wrappers Angular/React + Arq + stories catalog + ownership.

| CE | Wrappers | Stories |
|----|----------|---------|
| `base-divider` | Native* / ArqNative* | catalog.stories |
| `base-progress` | Native* / ArqNative* | catalog.stories |
| `base-chip` | Native* / ArqNative* | catalog.stories |

## Residual → F55

Select, Icon, Pagination, Filter bar — planes
[F55-A1](../plans/rounds/plans-55-fifty-five-round/1750000120000-f55-lit-wave-select-icon-pagination.md) /
[F55-A2](../plans/rounds/plans-55-fifty-five-round/1750000121000-f55-lit-wave-filter-bar-residual.md).
Re-evaluar licencia si aparece repo externo con Code Connect / Storybook export.
