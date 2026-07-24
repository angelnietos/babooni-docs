# F85-C — Portability hardening

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Hacer el harness de tests y seeds conscientes de `DATABASE_PROVIDER`.

## Acciones

- [ ] Extender `prisma-repo.harness.ts` para inyectar provider + URL por suite
- [ ] Targets `seed:mssql` / `seed:mysql` (o documentar skip hasta migrate real)
- [ ] Ampliar smoke specs de adapters si faltan edge cases

## Criterios

- [ ] Suites kernel verdes con override `DATABASE_PROVIDER`
- [ ] Runbook [database-providers.md](../../../runbooks/database-providers.md) actualizado
