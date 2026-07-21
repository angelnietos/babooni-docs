# F46-A1 — Corregir imports rotos en `base-shared`

## Estado

completado

## Objetivo

Corregir imports inválidos en `template-nav.spec.ts` que causan fallo de
`tsc --noEmit` en `base-shared`.

## Tareas

1. Abrir `libs/base/shared/src/lib/shell/template-nav.spec.ts`.
2. Reemplazar:
   - `import { UserRole } from './enums';` → `import { UserRole } from '../rbac/enums';`
   - `import { PERMISSIONS } from './permissions';` → `import { PERMISSIONS } from '../rbac/permissions';`
3. Ejecutar `pnpm nx typecheck base-shared` para confirmar que pasa.

## Criterios de aceptación

- [ ] `base-shared:typecheck` pasa sin errores.
- [ ] No se modifican archivos fuera de `libs/base/shared/src/lib/shell/`.
