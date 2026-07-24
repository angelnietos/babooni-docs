# F85-A — Real MSSQL/MySQL migrate deploy + CI matrix

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Pasar de `prisma validate` en shadow schemas a **migrate deploy** / seeds
ejecutables y documentar CI matrix no bloqueante (motor monorepo).

## Acciones

- [ ] Definir historial de migraciones por motor (o expand/contract desde Postgres)
- [ ] Scripts `migrate:deploy:mssql` / `migrate:deploy:mysql` (o flags schema)
- [ ] Docker compose snippets SqlServer + MySQL en [database-providers.md](../../../runbooks/database-providers.md)
- [ ] CI job opcional `DATABASE_PROVIDER=sqlserver|mysql` (no bloquea main)

## Criterios

- [ ] Al menos un path MSSQL smoke en docs/CI
- [ ] `pnpm check:db-migrations` sigue verde
