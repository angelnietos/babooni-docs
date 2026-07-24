<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F83-A1 — Provider detection strategy</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F83" src="https://img.shields.io/badge/round-F83-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · 2026-07-24

## Objetivo

Detectar el proveedor de base de datos desde `DATABASE_URL` sin tocar código de dominio, y centralizar la lógica en `libs/base/backend/src/lib/platform/kernel/prisma/`.

## Acciones

- [ ] Crear `resolve-database-provider.ts` con tabla de mapeo esquema → provider:
  - `postgresql://`, `postgres://` → `postgresql`
  - `mssql://`, `sqlserver://`, `jdbc:sqlserver://` → `sqlserver`
  - `mysql://`, `mariadb://` → `mysql`
  - `file://` → `sqlite`
  - Sin esquema o desconocido → lanzar error claro con `VALID_DATABASE_URLS` en docs
- [ ] Extender `resolveDatabaseUrlOrDefault` para devolver `{ url, provider }` (o añadir `resolveDatabaseProvider(url)` paralelo).
- [ ] Añadir `DATABASE_PROVIDER` env opcional (override explícito) con validación tipo enum.
- [ ] Mover lógica actual de Postgres-only a un helper `isPostgres(provider)` para compatibilidad backwards.
- [ ] Tests unitarios del parser con URLs canónicas de cada proveedor y casos edge (puertos, parámetros query, credenciales con `@`, pwds con `:`, host IPv6, SSL).
- [ ] Documentar formato soportado en `docs/runbooks/database-migrations.md`.

## Criterios

- [ ] `resolveDatabaseProvider` cubre Postgres, SqlServer, MySql, MariaDB, Sqlite.
- [ ] Error si el provider no se reconoce (no silent-fallback a Postgres).
- [ ] `DATABASE_PROVIDER` override tiene prioridad sobre detección por URL.
- [ ] Tests de parseo verde en CI.
