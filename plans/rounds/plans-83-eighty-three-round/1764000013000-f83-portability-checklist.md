<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F83-D1 — Portability checklist + migrations</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F83" src="https://img.shields.io/badge/round-F83-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · espera F83-C1

## Objetivo

Convertir la portabilidad en contrato verificable: smoke tests, seeds, checklist CI y runbook por proveedor.

## Acciones

- [ ] Añadir `tests/backend/adapters/prisma-adapter.smoke.spec.ts`:
  - `it('resuelve provider desde URL')` para Postgres, SqlServer, MySql.
  - `it('instancia PrismaClient con adapter correcto')` mockeado.
  - `it('conecta y desconecta graciosamente si no hay DB')` (green warning).
- [ ] Ampliar `prisma-repo.harness.ts` para inyectar `DATABASE_PROVIDER` + `DATABASE_URL` por test, aislando suites.
- [ ] Añadir `scripts/db-portability-check.mjs`:
  - Lee `schema.prisma`, extrae `provider`, cruza con lista blanca.
  - Verifica que cada dominio `*-prisma.repository` no usa sintaxis Postgres-specific sin flag.
- [ ] `docs/runbooks/database-providers.md`:
  - Setup local por proveedor (docker-compose + envs).
  - Migraciones y seeds por proveedor.
  - Troubleshooting (conexión, collation en SqlServer, charset en MySql, length limits).
- [ ] CI opcional: matrix `DATABASE_PROVIDER=postgresql` (default) + `mssql` y `mysql` en jobs separados sin bloquear main.
- [ ] ADR F83-D1 (opcional) si se decide policy de soporte: postgres primary, otros best-effort.

## Criterios

- [ ] Smoke specs verde con cada provider mockeado.
- [ ] `npm run check:db-portability` (o script equivalente) pasa en main.
- [ ] Runbook documenta setup completo de cada proveedor.
- [ ] No hay imports directos de `@prisma/adapter-pg` en dominios (solo en kernel prisma).

## Notas

- El objetivo no es feature paridad 100% entre motores; es *cambiar de base sin reimplementar features*.
- Proveedores “sin Microsoft” como MySQL/MariaDB/Spanner/PlanetScale/SQLite deben poder ejecutar la app (quizás con JSON-as-string, sin arrays, uuid como string).
