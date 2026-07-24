<p align="center">
  <img src="../../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Ideauto · Migration (operativo)</h1>

<p align="center">
  <b>Checklist de ejecución</b> · Recalls_v2 → <code>clientes/ideauto/recalls</code>
</p>

<p align="center">
  <img alt="estado" src="https://img.shields.io/badge/estado-activa-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Ideauto" src="https://img.shields.io/badge/hub-Ideauto-0f766e?style=flat-square" /></a>
  <a href="../recalls/"><img alt="narrativa" src="https://img.shields.io/badge/doc-recalls-0d5f59?style=flat-square" /></a>
</p>

Esta carpeta es el **plan operativo de migración Ideauto** (tickets / checklist).  
La narrativa de producto vive en [`docs/ideauto/recalls/`](../recalls/).  
Las rondas Nx de **motor monorepo** (DB adapters, SaaS, etc.) viven en [`docs/plans/`](../../plans/) — **no** mezclar.

> F84 (plan strangler) cerrada — solo en git. La ejecución de producto continúa aquí.

---

## Índice

| Doc | Contenido |
|-----|-----------|
| [README](./README.md) | Este hub |
| [m0-m6-execution.md](./m0-m6-execution.md) | Checklist milestones M0–M6 + gates |
| [recalls/](../recalls/) | Narrativa (why, strategy, domain-map, stakeholders) |
| [runbooks/recalls-migration.md](../../runbooks/recalls-migration.md) | Runbook ops |
| [ADR 0013](../../adr/adr-0013-recalls-strangler-migration.md) | Strangler |

---

## Destino técnico

```
apps/clientes/ideauto/recalls/{backend,frontend}
libs/clientes/ideauto/  →  @ideauto/*
tag: layer:clientes
```

**No** `apps/productos-saas/` · **No** `@saas/*`.

---

## Dependencias de plataforma

| Pieza | Doc |
|-------|-----|
| Prisma multi-provider / MSSQL | [database-providers.md](../../runbooks/database-providers.md) · [ADR 0014](../../adr/adr-0014-database-provider-portability.md) |
| Nuevo cliente (patrón) | [nuevo-cliente-checklist.md](../../clientes/nuevo-cliente-checklist.md) |
| Ronda motor monorepo (activa) | [plans-85](../../plans/rounds/plans-85-eighty-five-round/) |

---

## Checklist rápido

- [ ] M0 scaffold verde (apps + libs `@ideauto/*`)
- [ ] M1–M6 según [m0-m6-execution.md](./m0-m6-execution.md) y [recalls/milestones.md](../recalls/milestones.md)
- [ ] Boundaries `layer:clientes` sin imports a `@arquetipos/*` / `@saas/*`
- [ ] Runbook alineado con estado real
