# Ideauto migration — ejecución M0–M6

Estado: **activa** · 2026-07-24

## Objetivo

Ejecutar el strangler de Recalls_v2 como producto cliente Ideauto
(`apps/clientes/ideauto/recalls`, `@ideauto/*`), siguiendo la narrativa en
[`docs/ideauto/recalls/`](../recalls/).

## Acciones

- [ ] Avanzar milestones M0–M6 según [milestones.md](../recalls/milestones.md)
- [ ] Mantener boundaries `layer:clientes` (no `@saas/*`, no `@arquetipos/*`)
- [ ] Wire Prisma → MSSQL vía adapters del kernel / [ADR 0014](../../adr/adr-0014-database-provider-portability.md) cuando toque DB legacy
- [ ] Gates Nx: typecheck / test / layout en libs `@ideauto/*`

## Criterios

- [ ] Runbook [recalls-migration.md](../../runbooks/recalls-migration.md) refleja estado real
- [ ] Docs en `docs/ideauto/recalls/` y esta carpeta sin links rotos a rondas borradas

## Enlaces

- Hub migración: [README](./README.md)
- Narrativa: [recalls/](../recalls/)
- [ADR 0013](../../adr/adr-0013-recalls-strangler-migration.md)
- [nuevo-cliente-checklist](../../clientes/nuevo-cliente-checklist.md)
