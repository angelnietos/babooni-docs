# Plans — cuadragésima segunda ronda (F42: cierre de deuda remanente F41)

**Creado:** 2026-07-20  
**Estado:** Iniciada  
**Precedente:** [F41](../plans-41-forty-one-round/README.md) (deduplicación + unificación de convenciones)

> F41 cerró la deuda principal de duplicación y unificó convenciones, pero dejó
> trabajo remanente: thick domains sin migrar a `TenantScopedCqrsFacade`, specs
> de controllers y repositorios que no usan los harness creados, y casts laxos
> en tests de backend.

## Deuda técnica a cerrar

| # | Deuda | Plan | Prioridad |
|---|-------|------|-----------|
| 1 | `resolveTenant()` duplicado en `clients`/`users`/`settings` (3×3 líneas) | F42-B1 | Alta |
| 2 | Controller specs repiten `overrideGuard` + `req as any` (9 specs) | F42-B2 | Media |
| 3 | Repo specs repiten `makeCrudDelegate`/`loadCtx`/`buildRepo` (6 specs) | F42-B3 | Media |
| 4 | `as any` en specs de backend (controllers, repos, services) | F42-F1 | Baja |
| 5 | Imports `../../../../` prohibidos por lint F41 en `domains/**` | F42-B4 | Media |

## Progreso por plan

| ID | Documento | Prioridad | Estado |
|----|-----------|-----------|--------|
| F42-B1 | [1784811000000-f42-migrate-thick-domains-to-facade.md](./1784811000000-f42-migrate-thick-domains-to-facade.md) | Alta | Pendiente |
| F42-B2 | [1784812000000-f42-migrate-controller-specs-to-harness.md](./1784812000000-f42-migrate-controller-specs-to-harness.md) | Media | Pendiente |
| F42-B3 | [1784813000000-f42-migrate-repo-specs-to-harness.md](./1784813000000-f42-migrate-repo-specs-to-harness.md) | Media | Pendiente |
| F42-F1 | [1784814000000-f42-cleanup-any-casts-in-backend-specs.md](./1784814000000-f42-cleanup-any-casts-in-backend-specs.md) | Baja | Pendiente |
| F42-B4 | [1784815000000-f42-eliminate-deep-relative-imports.md](./1784815000000-f42-eliminate-deep-relative-imports.md) | Media | Pendiente |
