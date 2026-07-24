# F86-C — Kernel reliability

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Cerrar huecos de fiabilidad del motor tras F85: seeds por provider, health
probes conscientes de DB, smoke estable.

## Acciones

- [ ] Implementar seeds reales o documentar expand/contract + skip policy (`seed:mssql` / `seed:mysql`)
- [ ] Priorizar **SQL Server** en seeds/smoke (clientes Microsoft — ver [F86-G](1766000007000-f86-microsoft-azure-data-plane.md))
- [ ] Health/readiness: fallar claro si adapter/provider no conecta
- [ ] Ampliar smoke backend con override `DATABASE_PROVIDER` donde sea viable
- [ ] Revisar outbox/Kafka/idempotency contracts no acoplados a Postgres-only APIs

## Criterios

- [ ] Runbook [database-providers.md](../../../runbooks/database-providers.md) sin stubs engañosos
- [ ] Path MSSQL usable en smoke o documentado como blocked-by F86-G
- [ ] `pnpm check:db-*` + typecheck kernel verdes
