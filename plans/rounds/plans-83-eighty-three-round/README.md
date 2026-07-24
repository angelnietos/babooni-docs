<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F83 — Database provider portability + adapters</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-82-eighty-two-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F82-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Activa** · apertura 2026-07-24

> Eje: desacoplar apps/libs de PostgreSQL como proveedor único de datos mediante adapters de conexión, detección por URL y esquemas/migraciones portables para M$-SQL Server, MySQL/MariaDB y otros motores comunes sin Microsoft también.

## Contexto

Hoy el backend arquetipos está atado a PostgreSQL:

1. `datasource db { provider = "postgresql" }` hardcodeado en `libs/base/backend/prisma/{single,multi}/schema.prisma`.
2. Servicios Nest usan `PrismaPg` (`@prisma/adapter-pg`) directamente: `prisma-single.service.ts`, `prisma-multi.service.ts`.
3. Migraciones, índices compuestos y tipos usan sintaxis PostgreSQL (`cuid()`, `@@index`, `@db.Date`, JSON, arrays, `Cascade`, …).
4. No existe detección de provider desde `DATABASE_URL` ni fallback por env.

Cliente requiere ejecutar la app sobre **SQL Server** (y potencialmente MySQL/MariaDB) sin cambiar dominio ni features.

## Objetivos clave

1. Proveedor configurable por URL (`DATABASE_URL`) sin tocar dominio.
2. Adapter de conexión parametrizado (Pg ⇒ Mssql ⇒ Mysql, con fallback).
3. Migraciones/DDL compatibles con cada motor (o guías de parseo específica).
4. Repos `*-prisma.repository` sin referencias directas a proveedor.
5. Tests/harness multi-provider para verificar conectividad y seam migrations.

## Planes detallados

| ID | Plan | Foco |
|----|------|------|
| F83-A1 | [Provider detection strategy](1764000010000-f83-provider-detection.md) | Parseo `DATABASE_URL`, envs, fallback |
| F83-B1 | [Adapter layer + service abstraction](1764000011000-f83-adapter-layer.md) | Servicios `PrismaMultiService` / `PrismaSingleService` parametrizados |
| F83-C1 | [Multi-provider schemas + URL routing](1764000012000-f83-multi-provider-schemas.md) | Metadata `provider` por schema, índices, tipos, CLI flags |
| F83-D1 | [Portability checklist + migrations](1764000013000-f83-portability-checklist.md) | Smoke tests, seeds, migraciones idempotentes, guides |
| F83-E1 | [Docs, gates y cierre](1764000014000-f83-docs-gates-close.md) | Hub planes + ADR + layout doc |

## Checklist de cierre F83

- [ ] F83-A1 parseo URL/DDL detecta Pg/SqlServer/MySql y no rompe multi-tenant
- [ ] F83-B1 servicios arrancan con adapter parametrizado; tests unitarios dev fallback
- [ ] F83-C1 schemas + migraciones ejecutan en Pg (green) y tienen guías para SqlServer/MySql
- [ ] F83-D1 checklist portability publicado; CI smoke por provider opcional
- [ ] F83-E1 docs + hub; **F83 cerrada**

## Predecesora / Siguiente

- Predecesora: [F82](../plans-82-eighty-two-round/) (cerrada)
- Libs: `libs/base/backend/prisma/{single,multi}`, `libs/base/backend/src/lib/platform/kernel/prisma/`
- Domains: `libs/base/backend/src/lib/domains/*/adapters/persistence/*.prisma.repository`
