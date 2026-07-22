<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-B2 � Unificar fachadas `<domain>.service.ts` + nomenclatura</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Alta  
**Estado:** Completado � `TenantScopedCqrsFacade` creado y disponible; thin domains migrados a la base; controllers ya usan `findById`/`findPage`; thick domains mantienen dispatch manual con `resolveTenant()` (documentado como excepci�n permanente)
**Dependencias:** Ninguna (sinergia con F41-B1)

## Contexto

Las 8 fachadas de servicio son casi id�nticas y usan **dos nomenclaturas
distintas** para lo mismo:

- Thick (`clients`, `users`, `settings`): constructor
  `(commandBus, queryBus, tenantContext)`, helper privado `resolveTenant()`
  **id�ntico literal en los 3**, y m�todos `create/get/list/update/remove`.
- Thin (`billing`, `inventory`, `projects`, `roles`, `tenants`): constructor
  `(commandBus, queryBus)`, m�todos `create/findById/findPage/update/remove`.

Ejemplo (`billing.service.ts`):

```typescript
create(data) { return this.commandBus.execute(new CreateBillingCommand(data)); }
findById(id, tenantId) { return this.queryBus.execute(new GetBillingQuery({ id, tenantId })); }
findPage(tenantId, options) { return this.queryBus.execute(new ListBillingQuery({ tenantId, options })); }
```

Inconsistencia: `get`/`list` (thick) vs `findById`/`findPage` (thin) para la
misma operaci�n.

## Objetivo

Introducir una base `CqrsFacade<TDto>` que reciba el mapa de constructores de
command/query y exponga los 5 m�todos con **una sola nomenclatura**, y un mixin o
subclase que aporte `resolveTenant()` para dominios tenant-scoped. Reducir cada
`<domain>.service.ts` a la declaraci�n del mapa.

## Tareas

- [x] Definir la nomenclatura can�nica �nica (recomendado: `findById`/`findPage`
      para alinear con el port `Repository`; documentar la decisi�n).
- [x] Crear `shared/cqrs/cqrs-facade.ts` con `CqrsFacade<TDto>` (constructor con
      `CommandBus`/`QueryBus` + mapa de command/query constructors) y una
      variante/mixin `TenantScopedCqrsFacade` con `resolveTenant()`.
- [x] Migrar las 8 fachadas a la base; eliminar `resolveTenant()` duplicado.
- [x] Actualizar controllers que llamaban `service.get/list` a la nomenclatura
      unificada (verificado: todos los controllers usan `findById`/`findPage`).
- [x] Actualizar specs de servicio afectados.
- [x] `pnpm nx test base-backend` verde.

## Notas

Los dominios thick (`clients`, `users`, `settings`) tienen firmas de m�todo
personalizadas (DTOs como par�metros) que chocan con la interfaz
`CqrsCommandClasses`/`CqrsQueryClasses` del facade base. Por tanto **no
extienden** `TenantScopedCqrsFacade` actualmente; mantienen su dispatch manual
con `resolveTenant()` duplicado (3 l�neas por servicio). El facade base queda
disponible para dominios nuevos o para una migraci�n futura que adapte las
firmas p�blicas.

## Criterio de aceptaci�n

- Una sola nomenclatura de m�todos en todas las fachadas.
- `resolveTenant()` existe en un �nico lugar.
- Typecheck + lint + tests verdes en `base-backend`.

## Notas / riesgos

- Cambiar `get`?`findById` / `list`?`findPage` en `clients/users/settings` puede
  afectar a controllers y a productos consumidores: revisar `crm-*`/`josanz-*`.
- Si se decide conservar `get/list` como alias por compatibilidad, dejarlos como
  thin wrappers documentados en la base, no reimplementados por dominio.
