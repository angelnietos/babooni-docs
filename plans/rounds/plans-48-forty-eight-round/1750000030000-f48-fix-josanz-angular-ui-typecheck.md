# F48-A1 — Corregir typecheck de josanz-angular-ui

## Estado

completado

## Resultado

`pnpm nx typecheck josanz-angular-ui` pasa a verde. Se ajustaron
`tsconfig.{lib,spec}.json` para usar `module: ES2022` + `moduleResolution: bundler`,
se añadieron métodos faltantes en `AlertComponent` y `EmptyStateComponent`, y se
endurecieron los tipos en `catalog-theme.facade.ts` y `josanz-list-export.service.ts`.

## Objetivo

Llevar `pnpm nx typecheck josanz-angular-ui` a verde. Los errores actuales son:
- `moduleResolution` no resuelve `@angular/common/http`, `@angular/cdk/drag-drop`, `@angular/cdk/scrolling`.
- Propiedades faltantes en componentes (`cornerClass` en `AlertComponent`, `EmptyStateComponent`).
- Tipos `unknown` en `catalog-theme.facade.ts` y `josanz-list-export.service.ts`.

## Tareas

1. Abrir `libs/clientes/josanz/angular-ui/tsconfig.lib.json` y `tsconfig.spec.json`.
2. Verificar que `@angular/common`, `@angular/cdk` y `@angular/platform-browser` están en `dependencies`/`peerDependencies` del package.json del workspace.
3. Corregir moduleResolution a `node16`/`bundler` si corresponde, o añadir paths faltantes en `tsconfig.base.json`.
4. Corregir las propiedades faltantes en:
   - `alert/alert.component.ts`
   - `empty-state/empty-state.component.ts`
5. Añadir tipados explícitos en:
   - `services/catalog-theme.facade.ts`
   - `catalog/list-export/josanz-list-export.service.ts`
6. Ejecutar `pnpm nx typecheck josanz-angular-ui` tras cada batch.

## Criterios de aceptación

- [x] `pnpm nx typecheck josanz-angular-ui` verde.
- [x] Cero errores TS2307/TS2571/TS2339 en `libs/clientes/josanz/angular-ui/`.
