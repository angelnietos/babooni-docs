# F46-B1 — Corregir extends de tsconfig en libs Ionic

## Estado

listo para ejecutar

## Objetivo

Corregir la ruta `extends` de `tsconfig.lib.json` en libs Ionic que apunta
a `libs/tsconfig.base.json` (no existe) en lugar de la raíz
`tsconfig.base.json`.

## Tareas

1. Listar libs Ionic con `tsconfig.lib.json` que usan
   `"../../../../../../tsconfig.base.json"` como extends.
2. Corregir la ruta a `"../../../../../../../tsconfig.base.json"` (7 niveles)
   en los archivos afectados.
3. Ejecutar `pnpm nx typecheck base-ionic-dashboard-api` y
   `pnpm nx typecheck base-ionic-dashboard-data-access` para confirmar.

## Criterios de aceptación

- [ ] `base-ionic-dashboard-api:typecheck` pasa.
- [ ] `base-ionic-dashboard-data-access:typecheck` pasa.
- [ ] `base-ionic-dashboard-shell:typecheck` pasa.
- [ ] No se modifican archivos fuera de `libs/base/frontend/mobile/ionic/`.
