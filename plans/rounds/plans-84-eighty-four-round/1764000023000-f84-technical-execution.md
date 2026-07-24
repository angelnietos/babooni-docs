<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F84-D1 — Plan de ejecución técnica</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F84" src="https://img.shields.io/badge/round-F84-14b8a6?style=flat-square" /></a>
  <a href="../../../runbooks/recalls-migration.md"><img alt="runbook" src="https://img.shields.io/badge/runbook-recalls-0f766e?style=flat-square" /></a>
</p>

## Estado

**listo para ejecutar** · espera firmas F84-B1 / F84-C1

> Canónico operativo: [`docs/runbooks/recalls-migration.md`](../../../runbooks/recalls-migration.md).

---

## Objetivo

Pasos ejecutables por milestone, con comandos Nx y criterios de done.  
**No** es un dump del código legacy: es la obra sobre el monorepo.

---

## Pre-work (bloqueante)

1. **F83 verde** para MSSQL: `DATABASE_URL` sqlserver + adapter Prisma.
2. Decidir nombres Nx: `recalls-backend`, `recalls-frontend` (o slug `ideauto-recalls-*`).
3. Inventario de permisos authguard-core (matriz rol → ruta).
4. Backup MSSQL + procedimiento PITR antes de cualquier write dual.

```bash
# Verificar portabilidad DB (F83)
pnpm nx typecheck base-backend
# Scaffold (ajustar generator local si existe; preferir generadores workspace)
pnpm nx g @nx/nest:application recalls-backend --directory=apps/productos-saas/recalls/backend --no-interactive
pnpm nx g @nx/next:application recalls-frontend --directory=apps/productos-saas/recalls/frontend --no-interactive
```

> Preferir generadores **locales** del monorepo si existen (`nx-generate` skill). Paths deben respetar `apps/productos-saas/` y tags `layer:productos-saas`.

---

## M0 — Setup + adapters

**Por qué primero:** sin composition root y sin Prisma→MSSQL no hay strangler.

- [ ] Apps backend/frontend arrancan en monorepo.
- [ ] Libs seed: `recalls-shared` + un dominio piloto (`users` thin → `@base`).
- [ ] `DATABASE_URL` apunta a MSSQL de staging (introspect o schema mapeado).
- [ ] CI: `nx affected -t lint,test,typecheck,build` incluye proyectos recalls.

**Gate:** `pnpm nx typecheck recalls-backend` y `recalls-frontend` verdes.

---

## M1 — Auth

**Por qué:** la API legacy documenta listados/creación de usuarios **sin autenticación**. Migrar auth primero cierra el agujero y unifica identidad.

- [ ] Sustituir `/api/login`, confirmación email, recovery, change password.
- [ ] Retirar dependencia `@ideauto/authguard-core` del path migrado.
- [ ] Guards Nest + RBAC; **ningún** CRUD usuarios público.
- [ ] Seed superadmin staging.

**Gate:** E2E login + recovery; test negativo acceso anónimo a `/usuarios`.

---

## M2 — Campaigns / Waves

**Por qué:** es el corazón del producto (campañas de recall + oleadas).

- [ ] Dominio 4 capas FE + módulo Nest hex.
- [ ] Upload VINs (CSV/XLSX/TXT) via interceptor Nest + parsers tipados.
- [ ] Timeline oleadas postales/telemáticas.
- [ ] Proxy: rutas de campaña apuntan al nuevo backend; resto legacy.

**Gate:** CRUD campaña + subida VINs + crear oleada en staging.

---

## M3 — Budgets / Invoices / PDF

- [ ] Presupuestos + facturas; reutilizar patrones `@saas/invoices-*` sin mezclar Verifactu fiscal.
- [ ] Generación PDF/DOCX con templates producto; golden-file diff.
- [ ] Migrar backfills legacy solo como scripts one-shot documentados.

**Gate:** presupuesto aprobado + PDF firmado por negocio (paridad visual).

---

## M4 — DGT + Addresses

**Por qué es el milestone más peligroso:** sistema externo, XML, efectos legales.

- [ ] Client SOAP encapsulado **solo** en `@saas/dgt-api` (o adapter infra).
- [ ] Parallel-run: misma petición → legacy + nuevo; comparar respuesta.
- [ ] Addresses CRUD + normalización.
- [ ] Contract tests con fixtures XML (ya existen en legacy `tests/`).

**Gate:** consulta DGT E2E en staging; zero divergencia en casos golden.

---

## M5 — Admin / Reports / Workers

- [ ] Dashboard admin (stats) en features Next.
- [ ] Reports renting / VIN / certificates.
- [ ] Mover `node-schedule` a worker (BullMQ u homólogo monorepo).

**Gate:** export OK + job programado ejecuta fuera del API process.

---

## M6 — Legacy off

- [ ] Proxy 100% Arquetipos.
- [ ] Apagar PM2 legacy.
- [ ] Documentar runbook de emergencia (restore).
- [ ] Retirar feature flags.

**Gate:** 72h sin rollback + métricas de error estables.

---

## Verificación continua (cada PR de dominio)

```bash
pnpm nx typecheck recalls-backend
pnpm nx typecheck recalls-frontend
pnpm nx test recalls-backend
pnpm nx lint recalls-backend recalls-frontend
node tools/checks/check-lib-layout.mjs --strict
node tools/checks/check-frontend-conventions.mjs
```

---

## Criterios del plan

- [ ] Cada milestone = PR(s) con checklist verde.
- [ ] Sin dependencias circulares entre dominios.
- [ ] SOAP solo en capa `dgt` infra.
- [ ] Runbook biblia actualizado al cerrar cada M.

## Enlaces

- [Runbook](../../../runbooks/recalls-migration.md)
- [F84-B1](./1764000021000-f84-migration-strategy.md)
- [F84-C1](./1764000022000-f84-domain-mapping.md)
- [F83](../plans-83-eighty-three-round/README.md)
