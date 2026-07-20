# F42-B1 — Migrar thick domains a `TenantScopedCqrsFacade`

**Prioridad:** Alta  
**Estado:** Pendiente  
**Dependencias:** Ninguna

## Contexto

F41-B2 creó `TenantScopedCqrsFacade` y migró los 5 dominios thin (`billing`,
`inventory`, `projects`, `roles`, `tenants`), pero los 3 thick domains
(`clients`, `users`, `settings`) mantienen su dispatch manual con
`resolveTenant()` duplicado:

```typescript
// clients.service.ts, users.service.ts, settings.service.ts (idéntico)
private resolveTenant(tenantId?: string): string {
  return resolveTenantScope(tenantId, this.tenantContext.getTenantId());
}
```

Son 3 líneas idénticas × 3 servicios = 9 líneas de boilerplate que la base
ya elimina para thin domains.

El impedimento documentado en F41-B2 era que `clients`/`users`/`settings`
tienen firmas de método personalizadas (DTOs como parámetros) que chocan con
`CqrsCommandClasses`/`CqrsQueryClasses` del facade base. Sin embargo, los
métodos CRUD estándar (`create`, `findById`, `findPage`, `update`, `remove`)
sí pueden delegar al facade; los métodos extra (`findByEmail`, `sendInviteEmail`)
se quedan como overrides explícitos.

## Objetivo

Eliminar el `resolveTenant()` duplicado en los 3 thick domains haciendo que
hereden de `TenantScopedCqrsFacade` para los métodos CRUD estándar.

## Tareas

- [ ] Ajustar `TenantScopedCqrsFacade` (si es necesario) para acomodar las
      firmas personalizadas de `clients`/`users`/`settings` sin romper la
      interfaz base.
- [ ] Migrar `ClientsService` a herencia de `TenantScopedCqrsFacade`:
      - `create`, `findById`, `findPage`, `update`, `remove` → super()
      - `findByEmail` → override explícito que llama a `this.resolveTenant()`
- [ ] Migrar `UsersService` igual; `sendInviteEmail` queda como override.
- [ ] Migrar `SettingsService` igual.
- [ ] Actualizar specs de servicio afectados.
- [ ] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cero `private resolveTenant()` en `clients.service.ts`, `users.service.ts`,
  `settings.service.ts`.
- Los métodos extra (`findByEmail`, `sendInviteEmail`) siguen funcionando.
- Typecheck + lint + tests verdes en `base-backend`.

## Notas / riesgos

- Si las firmas personalizadas chocan con la interfaz base, considerar
  dejar los métodos CRUD en el facade y los extra como overrides sin
  modificar la interfaz base (open/closed).
