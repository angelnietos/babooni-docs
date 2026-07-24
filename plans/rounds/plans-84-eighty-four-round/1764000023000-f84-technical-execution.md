<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F84-D1 — Technical execution plan</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F84" src="https://img.shields.io/badge/round-F84-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar · espera F84-C1

## Objetivo

Plan detallado paso a paso por milestone y dominio, con comandos y gates.

## Pre-work

1. **Fork Nx workspace**: añadir `apps/recalls/backend` + `apps/recalls/frontend` (o integrar en `apps/productos-saas/`).
2. **Habilitar MSSQL** en arquetipos (F83-A1 → D1): `DATABASE_PROVIDER=mssql`, `@prisma/adapter-mssql`.
3. **Importar schema legacy**: mapear tablas MSSQL a modelos Prisma `schema.prisma` sin migraciones (introspect) o re-crear.
4. **DGT SOAP adapter**: `@base/soap-adapters` o npm `soap` en `dgt-api`.

## Milestone M0 — Setup + adapters base

```bash
pnpm nx g @nx/nest:app recalls-backend --directory=apps/recalls
pnpm nx g @nx/next:app recalls-frontend --directory=apps/recalls
pnpm nx build base-backend
pnpm nx build base-react
```

- [ ] `apps/recalls/backend` arranca con Prisma + MSSQL.
- [ ] `apps/recalls/frontend` SSR Next con `@base/react-api`.
- [ ] CI: `nx affected -t lint,test,typecheck,build` verde.

## Milestone M1 — Auth

- [ ] Migrar `/api/login`, `/api/usuarios`, `/api/confirmar-email`, `/api/recuperar-password`.
- [ ] Reemplazar `authguard-core` por guards Nest locales.
- [ ] Seed superadmin.
- Gate: login happy path + olvido contraseña.

## Milestone M2 — Campaigns/Waves

- [ ] Domain `campaigns` (4 capas) + `waves` como sub-feature.
- [ ] File upload VINs (CSV/XLSX) via `multer`/`express-fileupload` → Nest `FilesInterceptor`.
- [ ] Paginación y filtros (query builder legacy → Prisma select + `@base/pagination`).
- Gate: CRUD campañas, subida VINs, timeline oleadas.

## Milestone M3 — Budgets/Invoices + PDF

- [ ] Domain `budgets` + reusar `@base/invoices-*`.
- [ ] PDF generation: `@base/shared` utility (pdf-lib).
- [ ] Subida prefacturas/facturas.
 Gate: presupuesto aprobado + PDF generado.

## Milestone M4 — DGT + Addresses

- [ ] SOAP client封装 en `dgt-api`.
- [ ] Parallel run: legacy DGT sigue vivo; arquetipos envía copia.
- [ ] Addresses CRUD + normalización.
 Gate: consulta DGT exitosa end-to-end.

## Milestone M5 — Admin/Home + Reports

- [ ] Dashboard Next con `chart.js`.
- [ ] Reports renting/VINs/certificates.
- [ ] Tasks programadas (node-schedule → `@base/tasks` o BullMQ worker).
 Gate: dashboard carga + reportes exportan.

## Milestone M6 — Legacy off

- [ ] DNS/proxy enruta todo a arquetipos.
- [ ] Legacy apagado.
- [ ] Backup MSSQL point-in-time recovery validado.

## Criterios

- [ ] Cada milestone tiene PR propia con checklist verde.
- [ ] No hay dependencias circulares entre dominios.
- [ ] `nx typecheck recalls-backend` verde en cada milestone.

## Notas

- Si MSSQL no es mandatorio post-F83, considerar PostgreSQL en paralelo y sincronizar.
- SOAP DGT es único; mantener `soap` runtime solo en `dgt-api`.
