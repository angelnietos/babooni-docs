# Plans — cuadragésima tercera ronda (F43: consolidación facade + cleanup remanente)

**Creado:** 2026-07-21  
**Estado:** Iniciada  
**Precedente:** [F42](../plans-42-forty-two-round/README.md) (cierre deuda remanente F41)

> F42-B1 se fusionó con F43-A1: la migración a `TenantScopedCqrsFacade` se completó
> cambiando `CqrsFacade` y `TenantScopedCqrsFacade` a genéricos con 4 parámetros
> (`TDto`, `TCreateDto`, `TUpdateDto`, `TQueryDto`), eliminando los
> `// @ts-expect-error TS2416` y unificando `findPage(query)` en objeto.
>
> F43 continúa con el cleanup estructural restante: migración de specs a harness,
> limpieza de casts, eliminación de imports profundos y reducción de boilerplate
> en thick domains ahora que la firma base admite sobreescritura limpia.

## Deuda técnica a cerrar

| # | Deuda | Plan | Prioridad |
|---|-------|------|-----------|
| 1 | Controller specs repiten `overrideGuard` + `req as any` (9 specs) | F43-B1 | Media |
| 2 | Repo specs repiten `makeCrudDelegate`/`loadCtx`/`buildRepo` (6 specs) | F43-B2 | Media |
| 3 | `as any` en specs backend (controllers, repos, services) | F43-F1 | Baja |
| 4 | Imports `../../../../` prohibidos por lint en `domains/**` | F43-B3 | Media |
| 5 | Boilerplate `override update(id, data, tenantId)` en thick domains post-facade | F43-A2 | Baja |

## Progreso por plan

| ID | Documento | Prioridad | Estado |
|----|-----------|-----------|--------|
| F43-A2 | [1784869000000-f43-reduce-facade-override-boilerplate.md](./1784869000000-f43-reduce-facade-override-boilerplate.md) | Baja | Pendiente |
| F43-B1 | [1784868000000-f43-migrate-controller-specs-to-harness.md](./1784868000000-f43-migrate-controller-specs-to-harness.md) | Media | Pendiente |
| F43-B2 | [1784867000000-f43-migrate-repo-specs-to-harness.md](./1784867000000-f43-migrate-repo-specs-to-harness.md) | Media | Pendiente |
| F43-B3 | [1784866000000-f43-eliminate-deep-relative-imports.md](./1784866000000-f43-eliminate-deep-relative-imports.md) | Media | Pendiente |
| F43-F1 | [1784865000000-f43-cleanup-any-casts-in-backend-specs.md](./1784865000000-f43-cleanup-any-casts-in-backend-specs.md) | Baja | Pendiente |

## Orden recomendado

1. **F43-B1** — Migrar controller specs al harness (independiente, desbloquea F43-F1).
2. **F43-B2** — Migrar repo specs al harness (independiente).
3. **F43-B3** — Eliminar imports profundos (requiere consenso en barrel exports).
4. **F43-F1** — Limpiar `as any` residuales después de B1/B2.
5. **F43-A2** — Reducir boilerplate de overrides en thick domains (baja prioridad, mejora DX).

## Criterios de cierre

- [ ] Typecheck + lint + tests verdes en `base-backend`, `arquetipos-backend`, `clients-ms`.
- [ ] Cero `overrideGuard` inline en specs de controllers.
- [ ] Cero `loadCtx`/`buildRepo` inline en specs de repositorios.
- [ ] Cero `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`.
- [ ] Cero `// @ts-expect-error TS2416` en `libs/base/backend/`.

## Enlaces

- [F42 README](../plans-42-forty-two-round/README.md) — ronda anterior
- [F41 README](../plans-41-forty-one-round/README.md) — ronda F41 completada
- [docs/backend/backend-domain-convention.md](../backend/backend-domain-convention.md) — convención actualizada
- [docs/adr/adr-00xx-cqrs-nest.md](../adr/adr-00xx-cqrs-nest.md) — ADR CQRS Nest
- [docs/guides/ai-cqrs-policy.md](../guides/ai-cqrs-policy.md) — política AI CQRS
