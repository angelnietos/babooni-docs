<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Runbook — migración Recalls Ideauto (M0–M6)</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-84-eighty-four-round/"><img alt="F84" src="https://img.shields.io/badge/plan-F84-14b8a6?style=flat-square" /></a>
</p>

Cuándo usarlo: ejecutar milestones de migración Recalls_v2 → **`clientes/ideauto/recalls`**.

Contextos: [assessment](../architecture/recalls-v2-assessment.md) · [strategy](../architecture/recalls-migration-strategy.md) · [mapping](../architecture/recalls-domain-mapping.md).

---

## Precondiciones

1. F83 usable para MSSQL.
2. Backup MSSQL + PITR.
3. Matriz permisos legacy.
4. Proxy / flags staging.
5. Scaffold cliente: [nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md) (`--slug ideauto --scope ideauto`).

---

## M0 — Scaffold

```bash
node tools/scaffolds/scaffold-cliente-product.mjs --slug ideauto --scope ideauto --dry-run
node tools/scaffolds/scaffold-cliente-product.mjs --slug ideauto --scope ideauto

# Composition roots (ajustar generator local si existe)
pnpm nx g @nx/nest:application ideauto-recalls-backend --directory=apps/clientes/ideauto/recalls/backend --no-interactive
pnpm nx g @nx/next:application ideauto-recalls-frontend --directory=apps/clientes/ideauto/recalls/frontend --no-interactive

pnpm nx typecheck ideauto-recalls-backend
pnpm nx typecheck ideauto-recalls-frontend
node tools/checks/check-lib-layout.mjs --strict
```

**Done:** apps arrancan; Prisma→MSSQL staging; tags `layer:clientes`; **sin** `layer:productos-saas`.

---

## M1 — Auth

Login / recovery; sin acceso anónimo a usuarios; retirar authguard-core del slice.

## M2 — Campaigns / Waves

`@ideauto/campaigns-*`; upload VINs; proxy a Nest Ideauto.

## M3 — Budgets / Invoices / PDF

Paridad PDF + sign-off negocio.

## M4 — DGT

SOAP solo en `@ideauto/dgt-*`; parallel-run + contract tests XML.

## M5 — Reports / Admin / Workers

Exports + jobs fuera del API process.

## M6 — Cutover

100% tráfico a Ideauto recalls; legacy off; observación 72h.

---

## Verificación (cada PR)

```bash
pnpm nx affected -t lint,test,typecheck,build
node tools/checks/check-frontend-conventions.mjs
node tools/checks/check-ui-ownership.mjs
```

**Gate boundaries:** ningún import `@saas/*` / `@josanz/*` / `@arquetipos/*` desde código Ideauto.

---

## Rollback

1. Feature flag → Express legacy.
2. PITR MSSQL si corrupción.
3. No hot-fix Nest a medias en prod.

---

## Enlaces

- [F84-D1](../plans/rounds/plans-84-eighty-four-round/1764000023000-f84-technical-execution.md)
- [ADR 0013](../adr/adr-0013-recalls-strangler-migration.md)
