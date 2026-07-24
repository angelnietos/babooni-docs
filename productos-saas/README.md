<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Productos SaaS</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Productos vendibles aparte del ERP, bajo `apps/productos-saas/` y `@saas/*`.

| Doc | Contenido |
|-----|-----------|
| [productos-saas-extends-base.md](./productos-saas-extends-base.md) | Cómo SaaS extiende `@base/*` |
| [../architecture/recalls-v2-assessment.md](../architecture/recalls-v2-assessment.md) | Recalls_v2 — por qué migrar al monorepo |
| [../architecture/recalls-domain-mapping.md](../architecture/recalls-domain-mapping.md) | Recalls — mapeo dominio → `@saas/*` |
| [../runbooks/recalls-migration.md](../runbooks/recalls-migration.md) | Recalls — milestones M0–M6 |

### Roadmap producto

| Producto | Estado | Notas |
|----------|--------|-------|
| Verifactu CRM / worker | en monorepo | Apps + `@saas/*` actuales |
| **Recalls (Ideauto)** | migración **F84** | Legacy Express/Next externo → `apps/productos-saas/recalls` (strangler, [ADR 0013](../adr/adr-0013-recalls-strangler-migration.md)) |

También: [apps/productos-saas/README.md](../../apps/productos-saas/README.md), runbook [verifactu-demo-e2e.md](../runbooks/verifactu-demo-e2e.md).
