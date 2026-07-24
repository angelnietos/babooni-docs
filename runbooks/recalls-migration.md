<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Runbook — migración Recalls (M0–M6)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-84-eighty-four-round/"><img alt="F84" src="https://img.shields.io/badge/plan-F84-14b8a6?style=flat-square" /></a>
</p>

Cuándo usarlo: ejecutar o revisar un milestone de la migración Recalls_v2 → monorepo.

Contextos: [assessment](../architecture/recalls-v2-assessment.md) · [strategy](../architecture/recalls-migration-strategy.md) · [mapping](../architecture/recalls-domain-mapping.md).

---

## Precondiciones

1. [F83](../plans/rounds/plans-83-eighty-three-round/) usable para MSSQL (`DATABASE_URL` sqlserver).
2. Backup MSSQL + PITR verificado.
3. Matriz de permisos legacy inventariada.
4. Proxy / feature flags listos (staging).

---

## M0 — Scaffold

```bash
# Preferir generadores locales del monorepo si existen
pnpm nx g @nx/nest:application recalls-backend --directory=apps/productos-saas/recalls/backend --no-interactive
pnpm nx g @nx/next:application recalls-frontend --directory=apps/productos-saas/recalls/frontend --no-interactive

pnpm nx typecheck recalls-backend
pnpm nx typecheck recalls-frontend
node tools/checks/check-lib-layout.mjs --strict
```

**Done:** apps arrancan; Prisma conecta a MSSQL staging; tags `layer:productos-saas`.

---

## M1 — Auth

- Migrar login / recovery / confirmación.
- Eliminar acceso anónimo a CRUD usuarios en el path nuevo.
- Retirar `@ideauto/authguard-core` del slice migrado.

**Done:** E2E login + test negativo anónimo.

---

## M2 — Campaigns / Waves

- 4 capas FE + módulo Nest.
- Upload VINs tipado.
- Proxy enruta campañas al Nest.

**Done:** CRUD + VIN upload + oleada en staging.

---

## M3 — Budgets / Invoices / PDF

- Paridad PDF con golden files + sign-off negocio.

**Done:** presupuesto aprobado + PDF aceptado.

---

## M4 — DGT

- SOAP solo en adapter `dgt`.
- Parallel-run + contract tests XML.

**Done:** zero divergencia en fixtures golden.

---

## M5 — Reports / Admin / Workers

- Dashboard + exports.
- Jobs fuera del proceso HTTP.

**Done:** job schedule OK sin `node-schedule` en API.

---

## M6 — Cutover

- Tráfico 100% Arquetipos.
- Legacy PM2 off.
- Flags retirados tras periodo de observación.

**Done:** 72h estables sin rollback.

---

## Verificación (cada PR)

```bash
pnpm nx affected -t lint,test,typecheck,build
node tools/checks/check-frontend-conventions.mjs
node tools/checks/check-ui-ownership.mjs
```

---

## Rollback rápido

1. Feature flag → Express legacy para el path.
2. Si corrupción de datos: PITR MSSQL (ops).
3. No “arreglar a medias” el Nest en prod bajo presión.

---

## Enlaces

- [F84-D1](../plans/rounds/plans-84-eighty-four-round/1764000023000-f84-technical-execution.md)
- [ADR 0013](../adr/adr-0013-recalls-strangler-migration.md)
- [database-migrations.md](./database-migrations.md)
