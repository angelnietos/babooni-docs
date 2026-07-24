<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="72" alt="Arquetipos" />
</p>

<h1 align="center">Ideauto</h1>

<p align="center">
  <b>Producto cliente</b> · monorepo Arquetipos · scope <code>@ideauto/*</code>
</p>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <img alt="layer" src="https://img.shields.io/badge/layer-clientes-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./recalls/"><img alt="Recalls" src="https://img.shields.io/badge/migración-Recalls-0d5f59?style=flat-square" /></a>
</p>

**Ideauto** es un **cliente** del monorepo (misma regla que Josanz): vive en `apps/clientes/ideauto/` + `libs/clientes/ideauto/`, importa solo `@base/*`, y **no** es un producto SaaS (`@saas/*`).

---

## Productos

| Producto | Path app | Estado | Doc |
|----------|----------|--------|-----|
| **Recalls** | `apps/clientes/ideauto/recalls` | migración activa (strangler; F84 plan cerrado) | [recalls/](./recalls/) · [migration/](./migration/) |

---

## Audiencias

| Quién | Empieza aquí |
|-------|----------------|
| **Producto / negocio** | [recalls/why-migrate.md](./recalls/why-migrate.md) |
| **PM / delivery** | [recalls/milestones.md](./recalls/milestones.md) · [recalls/risks-and-rollback.md](./recalls/risks-and-rollback.md) |
| **Desarrollo** | [recalls/scope-and-placement.md](./recalls/scope-and-placement.md) · [recalls/domain-map.md](./recalls/domain-map.md) |
| **Cliente / stakeholders** | [recalls/stakeholders/client.md](./recalls/stakeholders/client.md) |

Guía rápida de lectura: [recalls/audience-guide.md](./recalls/audience-guide.md).

---

## Colocación en el monorepo

```
apps/clientes/ideauto/recalls/
├── backend/      # Nest composition root
└── frontend/     # Next.js composition root

libs/clientes/ideauto/
├── shared/       # @ideauto/shared
├── backend/      # @ideauto/backend
└── …             # dominios @ideauto/{domain}-*
```

| Regla | Valor |
|-------|-------|
| Slug | `ideauto` |
| npm | `@ideauto/*` |
| Tag | `layer:clientes` |
| Puede | `@base/*`, `@ideauto/*` |
| No puede | `@arquetipos/*`, `@saas/*`, `@josanz/*` |

Checklist genérico: [clientes/nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md).

---

## Plan de trabajo

Ejecución operativa: **[migration/](./migration/)** (M0–M6).

> Narrativa de producto: [recalls/](./recalls/).  
> F84 (checklist plan) cerrada — solo git.  
> Ronda Nx del **motor monorepo** (no Ideauto): [plans-85](../plans/rounds/plans-85-eighty-five-round/).

---

## Enlaces

- Biblia: [docs/README.md](../README.md)
- ADR strangler: [adr-0013](../adr/adr-0013-recalls-strangler-migration.md)
- DB MSSQL: [database-providers.md](../runbooks/database-providers.md) · [ADR 0014](../adr/adr-0014-database-provider-portability.md)
