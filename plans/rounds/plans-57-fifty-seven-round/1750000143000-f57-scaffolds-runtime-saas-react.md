# F57-B2 — Scaffolds: tags `runtime:*` + SaaS/React parity

## Estado

completado

## Objetivo

Que lo generado por scaffolds cumpla **hoy** las convenciones del monorepo
(tags `runtime:*` / `layer:*`, 4 capas FE, paths/exports) — no tags legacy
`scope:angular` / `scope:node` que `check-lib-layout --strict` rechaza.

## Entregables

1. Auditar y corregir tags en `gen-domain.mjs` (y scaffolds que copien project.json):
   - `runtime:backend|angular|react|isomorphic`
   - `layer:base|arquetipos|clientes|productos-saas`
   - Eliminar emisión de `scope:*` legacy.
2. Paridad React en scaffolds arquetipos/josanz donde hoy solo Angular esté completo
   (shell routes, features layout/pages/components mínimos, jest preset correcto).
3. Scope SaaS (`gen-domain --scope saas`): verificar package names `@saas/*`,
   layout `libs/productos-saas/…`, y al menos smoke typecheck del dominio generado en dry-run→apply en tmp o dominio `scaffoldprobe` borrable.
4. Post-scaffold checklist automática (script o pasos CLI):
   - `emit-frontend-tsconfig-paths` si aplica
   - `pnpm check:lib-layout` (warn o strict en CI del smoke E1)
   - recordatorio `pnpm install` / workspace deps

## Criterios de aceptación

- [x] Dominio generado (base o arquetipos) pasa `check-lib-layout --strict` en paths nuevos.
- [x] No se escriben tags `scope:*` en project.json nuevos.
- [x] React thin domain arquetipos generable con `--framework react|both`.
- [x] Docs de scaffold actualizadas con tags correctos.

## Verificación

```bash
# dry-run + inspección de tags en stdout/archivos temporales
pnpm scaffold:domain -- --scope base --domain scaffoldprobe --title Probe --dry-run
# apply en rama desechable o revert:
# pnpm check:lib-layout:strict
```
