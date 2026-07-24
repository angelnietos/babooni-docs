<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F86 — Official company platform engine</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <img alt="previa" src="https://img.shields.io/badge/prev-F85-14b8a6?style=flat-square" />
</p>

## Estado

**Activa** · apertura 2026-07-24

> Eje: este monorepo es la **base oficial** para cualquier app de la empresa.
> Endurecer el motor `@base/*`, contratos de bootstrap, **config server** FE/BE,
> plano **Microsoft/Azure** (SQL Server first-class), DX de nuevos productos y
> la barra de calidad que toda app debe respetar.
>
> **Previa F85 (cerrada):** migrate/validate multi-provider, SaaS → kernel
> adapters, harness `DATABASE_PROVIDER`, gates CI. Histórico git only.

## Contexto

F85 dejó el kernel Prisma portable (adapters únicos, shadow schemas, smoke CI
opcional). F86 sube de nivel: **producto plataforma** — toda app nace sobre el
mismo motor, con configuración centralizada y soporte serio a clientes
Microsoft (casi todos los despliegues corporativos).

## Objetivos clave

1. Contrato explícito “platform-as-product” (qué es obligatorio vs opt-in).
2. Bootstrap / checklist de nuevo producto alineado al motor.
3. Fiabilidad kernel (observabilidad, health, seeds multi-provider).
4. **Config server** compartido front + back (flags, branding, endpoints).
5. **SQL Server / Azure** como path de primera clase + adapters Azure.
6. DX + gates que protegen la base cuando crece el número de apps.
7. Docs/biblia sin contradicciones; cierre F86.

## Planes detallados

| ID | Plan | Foco |
|----|------|------|
| F86-A | [Platform-as-product contract](1766000001000-f86-platform-as-product-contract.md) | Manifesto + ADR + checklist obligatorio |
| F86-B | [New product bootstrap kit](1766000002000-f86-new-product-bootstrap.md) | Generators / checklist / plantillas |
| F86-C | [Kernel reliability](1766000003000-f86-kernel-reliability.md) | Seeds provider, health, smoke |
| F86-D | [Platform DX & quality bar](1766000004000-f86-platform-dx-quality-bar.md) | Gates, hygiene, CI matrix |
| F86-E | [Docs, gates y cierre](1766000005000-f86-docs-gates-close.md) | Hub + biblia |
| F86-F | [Platform config server](1766000006000-f86-platform-config-server.md) | Config FE+BE, flags, branding |
| F86-G | [Microsoft / Azure data plane](1766000007000-f86-microsoft-azure-data-plane.md) | SQL Server first-class + Azure adapters |

## Orden sugerido

```text
A (contrato) ─┬─► B (bootstrap)
              ├─► F (config server) ──► G (Azure App Config / Key Vault)
              ├─► C (reliability)   ──► G (Azure SQL / MSSQL seeds)
              └─► D (quality bar)
                              └─► E (cierre)
```

F y G pueden avanzar en paralelo tras A; G alimenta F (fuente remota de config)
y C (SQL Server real).

## Checklist de cierre F86

- [ ] F86-A contrato platform-as-product publicado
- [ ] F86-B bootstrap nuevo producto usable
- [ ] F86-C kernel reliability (seeds / health)
- [ ] F86-D quality bar + gates verdes
- [ ] F86-F config server consumible FE + BE
- [ ] F86-G SQL Server/Azure path documentado + ≥1 adapter Azure
- [ ] F86-E docs + hub; **F86 cerrada**

## Fuera de esta ronda

| Tema | Dónde |
|------|-------|
| Ideauto Recalls M0–M6 | [docs/ideauto/migration/](../../../ideauto/migration/) |
| Narrativa Recalls | [docs/ideauto/recalls/](../../../ideauto/recalls/) |

## Predecesoras

- F85 — cerrada (git only): multi-provider migrate path + SaaS adapters
- ADR: [0014](../../../adr/adr-0014-database-provider-portability.md)
- Kickoff doc: [platform-as-product.md](../../../architecture/platform-as-product.md)
