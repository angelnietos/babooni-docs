# Desarrollo — trabajar en Ideauto Recalls

Hub: [../README.md](../README.md)

## Paths

```
apps/clientes/ideauto/recalls/{backend,frontend}
libs/clientes/ideauto/  →  @ideauto/*
```

## Reglas duras

1. Solo imports `@base/*` y `@ideauto/*`.  
2. Nunca `@saas/*`, `@josanz/*`, `@arquetipos/*`.  
3. SOAP solo en `@ideauto/dgt-*`.  
4. Features nuevas: en monorepo, no en legacy.  
5. Preferir `pnpm nx typecheck <project>` y checks de layout.

## Día a día

```bash
pnpm nx typecheck ideauto-shared
pnpm nx typecheck ideauto-backend
node tools/checks/check-lib-layout.mjs --strict
```

Runbook: [recalls-migration.md](../../../runbooks/recalls-migration.md)  
Mapa: [domain-map.md](../domain-map.md)  
Colocación: [scope-and-placement.md](../scope-and-placement.md)  
Checklist cliente: [nuevo-cliente-checklist.md](../../../clientes/nuevo-cliente-checklist.md)

## Capas FE

`api → data-access → features ← ui` · `shell` lazy-load.  
Contrato: [frontend-domain-architecture-contract.md](../../../frontend/frontend-domain-architecture-contract.md).

## Migración operativa

Ejecución: [migration/](../../migration/).


