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
| **Recalls** | `apps/clientes/ideauto/recalls` | migración F84 (strangler) | [recalls/](./recalls/) |

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

## Plan de trabajo (Nx round)

Ronda operativa: [plans-84-eighty-four-round](../plans/rounds/plans-84-eighty-four-round/)  
GitHub: [plans-84 on main](https://github.com/Babooni-Technologies/arquetipos/tree/main/docs/plans/rounds/plans-84-eighty-four-round)

> Esta carpeta `docs/ideauto/` es la **doc de producto / migración** (humana).  
> `docs/plans/rounds/plans-84-…` es el **ticket de ronda** (checklist de cierre).

---

## Enlaces

- Biblia: [docs/README.md](../README.md)
- ADR strangler: [adr-0013](../adr/adr-0013-recalls-strangler-migration.md)
- F83 (MSSQL): [plans-83](../plans/rounds/plans-83-eighty-three-round/)
