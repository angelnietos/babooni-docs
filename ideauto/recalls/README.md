<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Ideauto · Recalls — migración</h1>

<p align="center">
  <b>De Recalls_v2 legacy → <code>clientes/ideauto/recalls</code></b>
</p>

<p align="center">
  <img alt="estado" src="https://img.shields.io/badge/estado-en%20migración-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Ideauto" src="https://img.shields.io/badge/hub-Ideauto-0f766e?style=flat-square" /></a>
  <a href="../migration/"><img alt="migration" src="https://img.shields.io/badge/migration-0d5f59?style=flat-square" /></a>
</p>

Documentación **independiente** de la migración del producto Recalls (Ideauto) al monorepo Arquetipos. Pensada para producto, PM, desarrollo y stakeholders de cliente.

---

## En una frase

> Dejamos de mantener un Express/Next suelto (Recalls_v2) y reconstruimos Recalls como **producto cliente Ideauto** en el monorepo: Nest + Prisma + capas FE + gates CI — **sin** meterlo en SaaS.

---

## Índice

| Doc | Para quién | Contenido |
|-----|------------|-----------|
| [audience-guide.md](./audience-guide.md) | Todos | Qué leer según rol |
| [why-migrate.md](./why-migrate.md) | Producto · cliente · PM | **Por qué** migrar (negocio + riesgo) |
| [scope-and-placement.md](./scope-and-placement.md) | Devs · arquitectura | Dónde vive el código; qué no es |
| [legacy-vs-target.md](./legacy-vs-target.md) | Devs · PM | Comparativa side-by-side |
| [migration-strategy.md](./migration-strategy.md) | PM · arquitectura | Strangler, fases, flags |
| [domain-map.md](./domain-map.md) | Devs | Entidades → `@ideauto/*` |
| [milestones.md](./milestones.md) | PM · Devs | M0–M6, gates, estado |
| [risks-and-rollback.md](./risks-and-rollback.md) | PM · Ops | Riesgos y rollback |
| [glossary.md](./glossary.md) | Todos | Glosario |
| [stakeholders/product.md](./stakeholders/product.md) | Producto | Decisiones de negocio |
| [stakeholders/pm.md](./stakeholders/pm.md) | PM | Delivery y dependencias |
| [stakeholders/developers.md](./stakeholders/developers.md) | Devs | Cómo trabajar en el monorepo |
| [stakeholders/client.md](./stakeholders/client.md) | Cliente Ideauto | Impacto y garantías |

---

## Destino técnico (resumen)

```
apps/clientes/ideauto/recalls/{backend,frontend}
libs/clientes/ideauto/  →  @ideauto/*
tag: layer:clientes
```

**No** `apps/productos-saas/` · **No** `@saas/*`.

---

## Plan de ronda

La checklist operativa F84 quedó **cerrada** (solo en git). Ejecución: **[migration](../migration/)**.

| Tema | Doc aquí |
|------|----------|
| Por qué / comparativa | [why-migrate](./why-migrate.md) · [legacy-vs-target](./legacy-vs-target.md) |
| Strategy strangler | [migration-strategy](./migration-strategy.md) |
| Mapeo dominio | [domain-map](./domain-map.md) |
| Milestones M0–M6 | [milestones](./milestones.md) |

ADR: [0013 — strangler Ideauto](../../adr/adr-0013-recalls-strangler-migration.md)  
Runbook ops: [runbooks/recalls-migration.md](../../runbooks/recalls-migration.md)

---

## Estado actual

| Milestone | Estado |
|-----------|--------|
| **M0** Setup scaffold Ideauto | en curso |
| M1 Auth | pendiente |
| M2 Campaigns / Waves | pendiente |
| M3 Budgets / PDF | pendiente |
| M4 DGT | pendiente |
| M5 Reports / Workers | pendiente |
| M6 Cutover | pendiente |

Detalle: [milestones.md](./milestones.md).
