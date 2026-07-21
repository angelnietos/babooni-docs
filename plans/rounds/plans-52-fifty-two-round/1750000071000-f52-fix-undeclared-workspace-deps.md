# F52-A2 — Cerrar `check:workspace-deps:strict` (undeclared imports)

## Estado

listo para ejecutar

## Objetivo

`pnpm check:workspace-deps:strict` falla con **~22 violaciones** preexistentes
(nota en F51-A1). Imports `@base/*` / `@josanz/*` / `@arquetipos/*` / `@saas/*`
sin `workspace:*` en el `package.json` del consumidor rompen el grafo pnpm y
bloquean hygiene estricto.

## Inventario (snapshot al abrir F52 — revalidar al ejecutar)

| Consumidor | Import undeclared (ejemplos) |
|------------|------------------------------|
| `@base/shared-store` | `@base/react` (spec) |
| `@base/ionic-auth-shell` | `@base/ionic-clients-shell`, `@base/ionic-dashboard-shell` |
| `@base/react-audit-data-access`, `@base/react-store` | `@base/shared` |
| `@josanz/documents-features` | `@josanz/angular` |
| `@josanz/settings-api` | `@base/shared` |
| `@arquetipos/arquetipos-backend` | `@base/shared` |
| `@arquetipos/angular-*-features` | `@base/angular`, `@base/shared` |
| `@arquetipos/react-*-features` / apps react | `@base/react-ui`, `@base/react-store`, `@base/shared` |
| `@saas/verifactu-crm-api` | `@base/shared` (seed) |

## Tareas

1. Re-ejecutar `pnpm check:workspace-deps:strict` y pegar lista actualizada en Resultado.
2. Por cada violación: `pnpm add <pkg> --filter <consumer> --workspace` (o editar
   `package.json` + `pnpm install`).
3. Specs: preferir declarar dep de test **o** mover import a fixture que no
   cruce capas indebidas.
4. Tras cambios: `pnpm hygiene` (o al menos workspace-deps + lint boundaries
   en proyectos tocados).
5. Nota corta en [pnpm-layout.md](../../../runbooks/pnpm-layout.md) /
   [workspace-packages.md](../../../frontend/workspace-packages.md) si el flujo
   de sync no está claro.

## Criterios de aceptación

- [ ] `pnpm check:workspace-deps:strict` exit 0.
- [ ] Sin nuevas violaciones de `@nx/enforce-module-boundaries` en libs tocadas.
- [ ] Lockfile actualizado de forma coherente (un solo `pnpm install` limpio).
