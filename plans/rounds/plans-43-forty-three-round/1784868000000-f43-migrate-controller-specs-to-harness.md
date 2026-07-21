# F43-B1 — Migrar controller specs a `buildControllerTestModule`

**Prioridad:** Media  
**Estado:** Pendiente  
**Dependencias:** Ninguna

## Contexto

F41-B5 creó `shared/testing/controller.harness.ts` con `buildControllerTestModule()`
pero **no se migraron** los 9 controller specs existentes. Siguen repitiendo:

```typescript
const req = { tenantId: 't1', user: { tenantId: 't1' } } as any;
// ...
.overrideGuard(JwtAuthGuard).useValue({ canActivate: () => true })
.overrideGuard(TenantGuard).useValue({ canActivate: () => true })
.overrideGuard(RolesGuard).useValue({ canActivate: () => true })
```

Archivos afectados:
- `domains/audit/adapters/http/audit.controller.spec.ts`
- `domains/billing/adapters/http/billing.controller.spec.ts`
- `domains/clients/adapters/http/clients.controller.spec.ts`
- `domains/inventory/adapters/http/inventory.controller.spec.ts`
- `domains/projects/adapters/http/projects.controller.spec.ts`
- `domains/roles/adapters/http/roles.controller.spec.ts`
- `domains/settings/adapters/http/settings.controller.spec.ts`
- `domains/tenants/adapters/http/tenants.controller.spec.ts`
- `domains/users/adapters/http/users.controller.spec.ts`

Nota: `clients-ms/src/app/clients.grpc.controller.spec.ts` ya se migró en F43-B1
y sirve como referencia del patrón esperado.

## Objetivo

Reducir cada spec a ~20 líneas usando el harness, eliminando el boilerplate
de guards y request.

## Tareas

- [ ] Ajustar `buildControllerTestModule` si es necesario para soportar
      el patrón de `req` como argumento de controller (algunos controllers
      reciben `req` como primer parámetro).
- [ ] Migrar los 9 controller specs al harness.
- [ ] Eliminar imports de guards en los specs migrados.
- [ ] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cada controller spec usa `buildControllerTestModule`.
- Cero `overrideGuard(...).useValue({ canActivate: () => true })` inline.
- Cero `as any` en `req` (tipar como `AuthedRequest` o Record).

## Notas / riesgos

- El harness devuelve `{ controller, req, service, moduleRef }`; si algún
  spec necesita acceso al `moduleRef` para comprobaciones adicionales,
  extender la interfaz de retorno.
