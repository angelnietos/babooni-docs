<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F83-C1 — Multi-provider schemas + migrations</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F83" src="https://img.shields.io/badge/round-F83-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · espera F83-A1 + F83-B1

## Objetivo

Soportar Prisma multi-provider en `{single,multi}/schema.prisma` sin perder capacidades Postgres, y proveer guías de migración para SqlServer / MySql.

## Acciones

- [ ] Añadir metadata `provider` por schema (environment-specific):
  - `prisma/single/schema.prisma` y `schema.postgres.prisma`, `schema.mssql.prisma`, `schema.mysql.prisma`.
  - Default: `schema.prisma` con `provider = "postgresql"`.
- [ ] Revisar features Postgres-only y marcar como `postgres` en docs:
  - `@db.Date`, arrays `String[]`, `Json`, `cuid()`, `@@index` compuesto, `onDelete: Cascade`.
  - Guía de mapeo por proveedor en `docs/frontend/database-provider-compat.md`.
- [ ] Añadir `prisma/migrations/README.md` con convenciones:
  - Nombre migraciones: `YYYYMMDDHHMMSS_{slug}`.
  - Idempotencia: cada SQL debe ser re-ejecutable; no usar `IF EXISTS` sólo en Pg.
- [ ] CLI helpers en `tools/checks/`:
  - `check-db-migrations.mjs`: verifica que cada carpeta `migrations/*/migration.sql` exists y es parseable por el provider activo.
  - `seed:pg` / `seed:mssql` / `seed:mysql` targets.
- [ ] Documentar cómo ejecutar migraciones por provider:
  ```bash
  DATABASE_PROVIDER=sqlserver npx prisma migrate deploy --schema prisma/single/schema.mssql.prisma
  ```
- [ ] Tests de integración (solo CI):
  - Levantar Postgres (docker actual).
  - Opcional SqlServer/MySql: documentar imágenes y envs sin bloquear CI por default.

## Criterios

- [ ] `npx prisma migrate dev` funciona en Postgres (green).
- [ ] `schema.mssql.prisma` y `schema.mysql.prisma` parsean con `prisma validate`.
- [ ] Guía de compatibilidad documenta qué features requieren proveedor específico.
- [ ] `node tools/checks/check-db-migrations.mjs` no falla en main.

## Riesgos

- `cuid()` no funciona en SqlServer fácilmente → mapeo a `NEWID()` / `uuid()` requiere generación custom.
- Arrays/JSON tienen dialectos distintos → modelar como tablas/json strings en proveedores no-PG.
- Índices compuestos multi-column y partiales pueden requerir sintaxis específica.
