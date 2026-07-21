# F43-A2 — Reducir boilerplate de overrides en thick domains post-facade

**Prioridad:** Baja  
**Estado:** Pendiente  
**Dependencias:** F43-A1 completada (facade genérica con 4 parámetros)

## Contexto

F43-A1 completó la migración de `clients`/`users`/`settings` a
`TenantScopedCqrsFacade<..., CreateXDto, UpdateXDto, ListXQueryDto>`.
Ahora los métodos CRUD estándar (`create`, `findById`, `findPage`, `update`,
`remove`) delegan al facade con sobreescritura explícita:

```typescript
override create(cmd: CreateClientDto): Promise<ClientDto> { ... }
override findPage(query: ListClientsQueryDto): Promise<Paged<ClientDto>> { ... }
override update(id: string, data: UpdateClientDto, tenantId?: string): Promise<ClientDto> { ... }
override remove(id: string, tenantId?: string): Promise<void> { ... }
```

Esto es correcto y necesario porque:
- Los thick domains construyen comandos personalizados (mapeo de DTOs).
- Los controllers pasan argumentos como `(id, body, tenantId)` en vez de
  `{ id, ...body, tenantId }` gracias a la nueva firma genérica.

Pero hay **oportunidad de reducción**: algunos métodos son casi idénticos
a los del facade y solo agregan 1-2 líneas de mapeo. Evaluar si el mapeo
justifica el override o si puede expresarse como factory de comandos.

## Objetivo

Reducir la cantidad de overrides verbosos en thick domains sin perder
claridad ni romper la convención de dispatchers personalizados.

## Análisis

### `clients.service.ts`
- `create(cmd)` → mapea `{ tenantId, name, email }` a `CreateClientCommand`.
  **No se puede eliminar** porque el comando no coincide con `CreateClientDto`.
- `findPage(query)` → mapea 7 campos a `ListClientsQuery`.
  **No se puede eliminar** porque `ListClientsQueryDto` tiene campos extra
  (`name`, `email`) no presentes en `QueryOptions`.
- `update(id, data, tenantId)` → mapea `{ id, tenantId, name, email }` a `UpdateClientCommand`.
  **No se puede eliminar** por el mismo motivo que `create`.

### `users.service.ts`
- Idéntico análisis: `create`/`findPage`/`update` mapean a comandos con campos
  específicos (`sendInviteEmail`, `roleId`, etc.).

### `settings.service.ts`
- `create` mapea `{ tenantId, key, value }` a `CreateSettingCommand`.
- `findPage` mapea a `ListSettingsQuery` (sin filtros extra, pero tenant scoping).
- `update` mapea `{ id, tenantId, value }` a `UpdateSettingCommand`.

## Tareas

- [ ] Documentar en `docs/backend/backend-domain-convention.md` la regla:
  > Si el comando/query del dominio **no coincide estructuralmente** con el
  > DTO HTTP, el override es obligatorio. Si coincide, delegar al facade.
- [ ] Evaluar si `settings.service.ts` puede eliminar `create`/`findPage`/`update`
      usando factories de comandos genéricos (riesgo: perder claridad del mapeo).
- [ ] Añadir helper `commandFromDto(dto, mapper)` si se decide generalizar.
- [ ] Actualizar ADR `adr-0009-cqrs-nest.md` con la convención de overrides.

## Criterio de aceptación

- Documentación actualizada con la regla de decisión.
- Si se añade helper, tiene tests y typecheck verde.
- No se reduce la claridad del mapeo DTO→comando en thick domains.

## Notas / riesgos

- **No forzar la eliminación de overrides.** El objetivo es documentar la
  convención, no reducir por reducir. Si el override es necesario, se queda.
- Si en el futuro se adopta un constructor genérico de comandos
  (`CommandFactory<TCreateDto>`), este plan se convierte en follow-up.
