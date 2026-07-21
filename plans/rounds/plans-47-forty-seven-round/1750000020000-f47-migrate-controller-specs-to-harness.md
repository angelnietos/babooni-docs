# F47-A1 — Migrar controller specs a `buildControllerTestModule`

## Estado

listo para ejecutar

## Objetivo

Completar la migración de los 9 controller specs de `base-backend` al harness
`buildControllerTestModule` creado en F41-B5, eliminando el boilerplate repetido
de guards y request mock.

## Tareas

1. Abrir `libs/base/backend/src/lib/domains/audit/adapters/http/audit.controller.spec.ts`.
2. Reemplazar el bootstrap manual del módulo por `buildControllerTestModule`.
3. Eliminar imports de `JwtAuthGuard`, `TenantGuard`, `RolesGuard` y el `req as any`.
4. Repetir para los 8 dominios restantes:
   - `billing`
   - `clients`
   - `inventory`
   - `projects`
   - `roles`
   - `settings`
   - `tenants`
   - `users`
5. Ejecutar `pnpm nx test base-backend` para confirmar que pasa.

## Criterios de aceptación

- [ ] Cada controller spec usa `buildControllerTestModule`.
- [ ] Cero `overrideGuard(...).useValue({ canActivate: () => true })` inline.
- [ ] Cero `as any` en `req`.
- [ ] `pnpm nx test base-backend` verde.
