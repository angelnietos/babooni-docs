<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F84-A1 — Comparative analysis</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F84" src="https://img.shields.io/badge/round-F84-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · 2026-07-24

## Objetivo

Publicar un informe side-by-side de `Recalls_v2` vs `arquetipos` que justifique migrar y priorice workstreams.

## Acciones

- [ ] **Inventario Recalls_v2** (leer + ejecutar scripts si existen):
  - Backend: Express 5, Sequelize 6, MSSQL/tedious, JWT, node-schedule, SOAP (dgt).
  - Frontend: Next 16, React 19, Redux Toolkit, Tailwind v4, next-intl, pnpm workspace.
  - Estructura: monolito cliente/servidor separado pero sin tooling Nx.
  - Auth: `@ideauto/authguard-core` (external) + custom middleware.
  - DB: `src/database/models/*.js` (25 models), migrations en `.sql` + `.cjs`.
  - Dominio: campaigns, waves, budgets, invoices, clients, contacts, dgt, addresses, reports.
- [ ] **Matriz comparativa**:
  - Framework backend: Express monolito vs NestJS modular.
  - ORM: Sequelize (MSSQL-only) vs Prisma (multi-provider).
  - Tipado: JS (JSDoc parcial) vs TS estricto.
  - Frontend layering: pages/components/hooks sin capas claras vs api/data-access/features.
  - Testing: Jest parcial (algunos `*.test.js`) vs Jest completo + coverage gates.
  - CI/CD: scripts sueltos (`pm2`, `ecosystem.config.cjs`) vs Nx affected + Nx Cloud.
  - Docs: API_DOCUMENTATION.md (estático) vs API platform docs + ADRs.
  - Multi-tenancy: no presente vs multi-tenant schema + tenant mode.
- [ ] **Deuda técnica**:
  - SQL migrations manuales (`.sql` + `.cjs`) sin schema único.
  - No hay barrel packages; imports relative profundos.
  - Logging con Winston pero sin correlación/contexto estandarizado.
  - Auditoría, PII encryption, RBAC no presentes.
- [ ] **Gap de features**:
  - DGT SOAP integration → puerto a `@base/soap-adapters` o domain `dgt` en arquetipos.
  - Generación PDFs/DOCX/XLSX → reutilizar `pdf-lib`, `handlebars`, `xlsx` en `@base/shared` o domain.
  - Scheduled tasks (node-schedule) → `@base/tasks` o CQRS workers.

## Criterios

- [ ] Documento `docs/plans/rounds/plans-84-eighty-four-round/1764000020000-f84-comparative-analysis.md` con tablas y bullets.
- [ ] Lista de workstreams priorizada (P0/P1/P2).
- [ ] Producto signa el mapeo antes de F84-C1.
