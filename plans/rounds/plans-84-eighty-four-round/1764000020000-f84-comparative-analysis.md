<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F84-A1 — Por qué migrar · análisis comparativo</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F84" src="https://img.shields.io/badge/round-F84-14b8a6?style=flat-square" /></a>
  <a href="../../../architecture/recalls-v2-assessment.md"><img alt="biblia" src="https://img.shields.io/badge/biblia-assessment-0f766e?style=flat-square" /></a>
</p>

## Estado

**completado (contenido en biblia)** · 2026-07-24 · colocación **cliente Ideauto**

> Canónico: [`docs/architecture/recalls-v2-assessment.md`](../../../architecture/recalls-v2-assessment.md).

---

## Objetivo

Justificar por qué dejar de construir “a lo Recalls_v2” y por qué el destino es **`clientes/ideauto/recalls`** (`@ideauto/*`), no SaaS.

---

## Tesis

> Recalls_v2 acopla dominio crítico (DGT, oleadas, VINs, facturación) a Express/JS sin capas y a un ORM de un solo proveedor. El monorepo ya resolvió eso para **productos cliente** (patrón Josanz): Nest hex, Prisma portable, 4 capas FE, gates CI — bajo `apps/clientes/{slug}` + `@slug/*`.

Ideauto es el **cliente**; Recalls es su producto. Misma regla que Josanz ≠ Verifactu.

---

## Matriz side-by-side (resumen)

| Dimensión | Recalls_v2 (hoy) | Destino Arquetipos |
|-----------|------------------|--------------------|
| Colocación | Dos repos sueltos | `apps/clientes/ideauto/recalls` + `libs/clientes/ideauto` |
| npm | sin scope monorepo | `@ideauto/*` · `layer:clientes` |
| Backend | Express 5 monolito | NestJS + hex + CQRS |
| Persistencia | Sequelize MSSQL only | Prisma + adapter MSSQL (F83) |
| Tipado BE | JavaScript | TypeScript estricto |
| Frontend | Next + RTK + atomic UI | Next (opt-in) + `api → data-access → features` |
| Auth | JWT + `@ideauto/authguard-core` | Keycloak / guards Nest |
| Jobs | `node-schedule` in-process | Worker / cola |
| CI | Scripts + PM2 | `nx affected` |

---

## Deuda que no se “parchea”

| Deuda | Mitigación en destino |
|-------|------------------------|
| Services flat (~166) | Módulos Nest en `@ideauto/backend` |
| Query builder casero | Prisma + repositorios |
| File paths por campaña | Storage port + `@ideauto/files-*` |
| SOAP DGT embebido | `@ideauto/dgt-*` adapter aislado |
| Redux + fetch en páginas | `@ideauto/*-data-access` facades |
| Atomic design como arquitectura | UI kit ≠ capas de dominio |

---

## Gaps de features

| Capacidad legacy | Destino |
|------------------|---------|
| SOAP DGT | `@ideauto/dgt-*` + `soap` |
| PDFs / XLSX / Handlebars | `@base` utilities + templates `@ideauto` |
| Oleadas postales/telemáticas | Subdominio waves bajo campaigns |
| Informes renting / VIN / certificates | `@ideauto/reports-*` |
| Subida masiva VINs | Nest interceptor + parsers tipados |

---

## Workstreams

| Prio | Workstream |
|------|------------|
| **P0** | Scaffold `clientes/ideauto` + Prisma MSSQL (F83) + auth |
| **P1** | Campaigns / waves + DGT adapter |
| **P2** | Budgets / invoices / PDF + reports + workers |

---

## Criterios

- [x] Inventario + matriz + P0/P1/P2.
- [x] Destino explícito: **no SaaS**.
- [ ] Producto confirma prioridades antes de M2.

## Enlaces

- [README F84](./README.md) · [Assessment](../../../architecture/recalls-v2-assessment.md) · [F84-B1](./1764000021000-f84-migration-strategy.md)
