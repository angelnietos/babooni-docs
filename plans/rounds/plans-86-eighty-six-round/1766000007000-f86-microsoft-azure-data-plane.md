# F86-G — Microsoft / Azure data plane (SQL Server first-class)

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Tratar el ecosistema **Microsoft** como path de primera clase en el motor: la
mayoría de clientes corporativos usan **SQL Server** y Azure. Tras F85 (adapters
+ shadow schemas + smoke), F86 debe dejar SQL Server / Azure listos para producto,
no solo “best-effort validate”.

## Contexto

- Kernel ya detecta `sqlserver` / `mssql` y crea `@prisma/adapter-mssql`.
- Postgres sigue siendo SoT de migraciones locales/CI.
- Falta: historia operativa Azure (connection strings, Entra ID, firewall),
  seeds/smoke reales MSSQL, y adapters de plataforma Azure más allá de Prisma.

## Alcance

### Database — SQL Server / Azure SQL

- [ ] Documentar connection strings Azure SQL (encrypt, `Authentication=Active
      Directory…`, managed identity) en [database-providers.md](../../../runbooks/database-providers.md)
- [ ] Ampliar factory si hace falta (parse URL / config object para
      `PrismaMssql` con opciones Azure)
- [ ] Path smoke CI/local estable para Azure SQL **o** SQL Server en compose
      (completar F85 stubs de `seed:mssql`)
- [ ] Guía expand/contract: Postgres SoT → deploy schema MSSQL en cliente
- [ ] Checklist “nuevo cliente Microsoft”: `DATABASE_PROVIDER=sqlserver` + URL Azure

### Azure platform adapters (ports en `@base/backend`)

Prioridad sugerida (YAGNI: ports primero, un adapter real por ítem):

| Capacidad | Port (ideal) | Adapter Microsoft |
|-----------|--------------|-------------------|
| Config remota | `ConfigSourcePort` (F86-F) | Azure App Configuration |
| Secrets | ya hay `KeyProvider` / secrets | Azure Key Vault |
| Blob / files | storage port (si no existe) | Azure Blob Storage |
| Identity | Keycloak primary; bridge | Microsoft Entra ID (OIDC) — doc + opt-in |
| Messaging (opt) | event bus | Service Bus / Event Hubs (solo si un cliente lo exige) |

### Out

- Reescribir dominio para T-SQL nativo
- Azure-only como default del monorepo (Postgres local sigue default DX)

## Acciones

- [ ] Matriz “cliente Microsoft” en platform-as-product (must/should)
- [ ] Runbook Azure SQL + ejemplos `.env`
- [ ] Adapter Key Vault **o** App Configuration cableado al config server (F86-F)
- [ ] Seed/smoke MSSQL real o política explícita documentada
- [ ] Tests de resolución de provider con URLs estilo Azure
- [ ] Actualizar ADR 0014 consecuencias / enlace F86-G

## Criterios

- [ ] Un producto cliente puede apuntar a Azure SQL solo con env + docs
- [ ] Ningún producto importa `@prisma/adapter-mssql` directo (`check:db-portability`)
- [ ] Al menos un adapter Azure (config o secrets) usable desde el kernel

## Relacionado

- [ADR 0014](../../../adr/adr-0014-database-provider-portability.md)
- [database-providers.md](../../../runbooks/database-providers.md)
- [F86-F config server](1766000006000-f86-platform-config-server.md)
- [F86-C reliability](1766000003000-f86-kernel-reliability.md)
