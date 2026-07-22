<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F47-A4 — Corregir typecheck de josanz-angular-ui</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Llevar `josanz-angular-ui` a verde corrigiendo los errores de typecheck
preexistentes que bloquean la validación completa del workspace.

## Errores identificados

1. `libs/clientes/josanz/angular-ui/src/lib/catalog/list-export/josanz-list-export.service.ts`
   - No encuentra `@angular/common/http` bajo `moduleResolution: "node"`.
   - Causa: tsconfig.lib.json usa `node` en vez de `nodenext`/`bundler`.
2. `libs/clientes/josanz/angular-ui/src/lib/catalog/status-kanban-board/status-kanban-board.ts`
   - No encuentra `@angular/cdk/drag-drop` ni `@angular/cdk/scrolling`.
   - Causa: mismo problema de moduleResolution + dependencia faltante en tsconfig.
3. `libs/clientes/josanz/angular-ui/src/lib/services/catalog-theme.facade.ts`
   - No encuentra `@angular/common/http`.
   - Causa: mismo problema.
4. `libs/clientes/josanz/angular-ui/src/lib/feedback/alert/alert.spec.ts`
   - Propiedad `cornerClass` no existe en `AlertComponent`.
5. `libs/clientes/josanz/angular-ui/src/lib/feedback/empty-state/empty-state.spec.ts`
   - Propiedad `cornerClass` no existe en `EmptyStateComponent`.
6. `libs/clientes/josanz/angular-ui/src/lib/overlays/modal/modal.spec.ts`
   - Argumento de tipo `string` no asignable a `(this: any, ...args: any[]) => unknown`.

## Tareas

1. Corregir `tsconfig.lib.json` de `josanz-angular-ui` para usar
   `"moduleResolution": "bundler"` (alineado con el resto de libs Angular).
2. Corregir los specs que acceden a `cornerClass` inexistente.
3. Corregir el tipo de argumento en `modal.spec.ts`.
4. Ejecutar `pnpm nx typecheck josanz-angular-ui` para confirmar que pasa.

## Criterios de aceptación

- [ ] `josanz-angular-ui:typecheck` pasa sin errores.
- [ ] No se modifican archivos fuera de `libs/clientes/josanz/angular-ui/`.
