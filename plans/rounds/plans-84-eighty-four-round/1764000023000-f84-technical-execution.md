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

**listo para ejecutar** · paths bajo **`clientes/ideauto/recalls`**

> Canónico: [`runbooks/recalls-migration.md`](../../../runbooks/recalls-migration.md).

---

## Pre-work

1. **F83 verde** para MSSQL.
2. Scaffold cliente Ideauto ([nuevo-cliente-checklist](../../../clientes/nuevo-cliente-checklist.md)):

```bash
# Vista previa
node tools/scaffolds/scaffold-cliente-product.mjs --slug ideauto --scope ideauto --dry-run

# Esqueleto libs (shared, backend, …)
node tools/scaffolds/scaffold-cliente-product.mjs --slug ideauto --scope ideauto
```

3. Apps composition roots:

```bash
# Ajustar generators locales si existen; paths canónicos:
# apps/clientes/ideauto/recalls/backend
# apps/clientes/ideauto/recalls/frontend
pnpm nx g @nx/nest:application ideauto-recalls-backend --directory=apps/clientes/ideauto/recalls/backend --no-interactive
pnpm nx g @nx/next:application ideauto-recalls-frontend --directory=apps/clientes/ideauto/recalls/frontend --no-interactive
```

4. Tags: `layer:clientes` (nunca `layer:productos-saas`).
5. Inventario permisos authguard-core + backup MSSQL.

---

## M0 — Setup

- [ ] `libs/clientes/ideauto/{shared,backend,…}` existen.
- [ ] Apps `…/ideauto/recalls/{backend,frontend}` arrancan.
- [ ] Prisma → MSSQL staging.
- [ ] `nx typecheck` verde en proyectos Ideauto.

## M1 — Auth

- [ ] Login / recovery / confirmación.
- [ ] Retirar `@ideauto/authguard-core` del path migrado.
- [ ] Sin CRUD usuarios anónimo.

## M2 — Campaigns / Waves

- [ ] `@ideauto/campaigns-*` (+ waves).
- [ ] Upload VINs.
- [ ] Proxy enruta campañas al Nest Ideauto.

## M3 — Budgets / Invoices / PDF

- [ ] `@ideauto/budgets-*` / invoices producto.
- [ ] Golden PDF + sign-off.

## M4 — DGT

- [ ] SOAP solo en `@ideauto/dgt-*`.
- [ ] Parallel-run + fixtures XML.

## M5 — Reports / Admin / Workers

- [ ] `@ideauto/reports-*` + admin features.
- [ ] Jobs fuera del proceso HTTP.

## M6 — Cutover

- [ ] 100% tráfico a `clientes/ideauto/recalls`.
- [ ] Legacy off + PITR validado.

---

## Verificación

```bash
pnpm nx typecheck ideauto-recalls-backend
pnpm nx typecheck ideauto-recalls-frontend
node tools/checks/check-lib-layout.mjs --strict
node tools/checks/check-frontend-conventions.mjs
```

## Criterios

- [ ] Cada M = PR(s) verdes.
- [ ] Sin imports `@saas/*` / `@josanz/*` / `@arquetipos/*` desde Ideauto.
- [ ] SOAP solo en capa dgt.

## Enlaces

- [Runbook](../../../runbooks/recalls-migration.md) · [F84-C1](./1764000022000-f84-domain-mapping.md)
