# Platform as product — contrato de base oficial

**Estado:** vigente (kickoff F86) · 2026-07-24

Este monorepo es la **base oficial** para cualquier aplicación de la empresa
(plantillas, productos cliente, SaaS). No es un “repo de demos”: el kernel
`@base/*` es el producto plataforma; las apps son composition roots.

## Principios

1. **Una base, muchas apps.** Dominio reutilizable en libs; apps solo componen.
2. **Capas npm inviolables.** `@base` ← `@arquetipos` / `@josanz` / `@ideauto` / `@saas`.
   Nunca al revés ni cruzar plantillas↔productos.
3. **Kernel owns infrastructure.** Prisma adapters, auth, tenancy, encryption,
   jobs, observability viven en `@base/backend` (o ports documentados). Los
   productos **no** construyen `PrismaPg` / drivers sueltos.
4. **Postgres primary en DX; SQL Server / Azure first-class en clientes.**
   Portabilidad vía `resolveAndCreateAdapter` + shadow schemas
   ([ADR 0014](../adr/adr-0014-database-provider-portability.md)). Plano Microsoft:
   [F86-G](../plans/rounds/plans-86-eighty-six-round/1766000007000-f86-microsoft-azure-data-plane.md).
5. **Config centralizada FE+BE.** Runtime config (flags, branding, endpoints) vía
   config server de plataforma — no solo `.env` por app
   ([F86-F](../plans/rounds/plans-86-eighty-six-round/1766000006000-f86-platform-config-server.md)).
6. **Frontend four layers.** `api → data-access → features ← ui`; `shell` lazy-loads.
7. **Opt-in beyond MVP.** Next / mobile / MF / microservicios son caminos
   documentados, no el default ([ADR 0008](../adr/adr-0008-platform-scope-vs-mvp-client.md)).

## Must / Should / May

| | Cliente (`@josanz/*`, `@ideauto/*`) | SaaS (`@saas/*`) | Plantilla (`@arquetipos/*`) |
|--|-------------------------------------|------------------|------------------------------|
| **Must** | Extender `@base/*`; checklist [nuevo-cliente](../clientes/nuevo-cliente-checklist.md); slug dominio ↔ HTTP; adapters DB vía kernel | Extender `@base/*`; adapters vía kernel | Thin re-export de `@base/*` |
| **Should** | UI marca en package de producto; facades por dominio; **Azure SQL / SQL Server** si el cliente es Microsoft; consumir config server | Reutilizar hex/ports base; config server | Mantener override points locales |
| **May** | Microservicio / mobile; Entra ID / Key Vault / App Configuration | Import selectivo `@josanz/*` documentado | Branding demo / themes |

## Contratos técnicos mínimos

| Área | Contrato |
|------|----------|
| DB URL | App `bootstrap-env` → `DATABASE_URL`; repos solo ports |
| Prisma | `resolveAndCreateAdapter` vía `@base/backend/prisma` |
| SQL Server / Azure | Provider `sqlserver`; docs Azure SQL en runbook providers |
| Config runtime | Config server (public keys al FE; secrets solo BE) |
| Auth / tenant | Módulos kernel; no reinventar guards |
| Layout libs | `node tools/checks/check-lib-layout.mjs --strict` |
| Portability | `pnpm check:db-portability` + `pnpm check:db-migrations` |
| Verify | Prefer `pnpm nx typecheck <project>` / `pnpm verify:affected` |

## Cómo abrir un producto nuevo

1. Leer [getting-started.md](../getting-started.md) + este doc.
2. Seguir [nuevo-cliente-checklist.md](../clientes/nuevo-cliente-checklist.md) o guía SaaS.
3. Componer módulos `@base/*` en la app; dominio propio solo donde aporten valor.
4. Pasar gates: layout, db-portability, typecheck, tests affected.

Detalle de planes: [F86](../plans/rounds/plans-86-eighty-six-round/).

## Relacionado

- [platform-vision.md](./platform-vision.md)
- [overview.md](./overview.md)
- [database-providers.md](../runbooks/database-providers.md)
- [AGENTS.md](../../AGENTS.md)
