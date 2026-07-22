<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-B4 � Alinear el port `Repository` y `TenantlessPrismaRepository`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Media  
**Estado:** Completado � `findPage` es obligatorio; `TenantlessPrismaRepository` alineado al port; fallbacks eliminados; cero `as unknown as XRepository`
**Dependencias:** Ninguna (desbloquea B1, B2, B3)

## Contexto

Dos problemas de ra�z en el port `shared/domain/repository.ts`:

1. **`findPage?` es opcional.** Esto obliga a un fallback de paginaci�n
   `if (repo.findPage) ... else { findAll + meta }` duplicado en 7+ sitios:
   `CrudService.findPage`, `list-settings.handler.ts`, y el bloque `ListX` de
   `billing`/`inventory`/`projects`/`roles`/`tenants`. Pero **`PrismaRepository`
   y `TenantlessPrismaRepository` siempre implementan `findPage`**, as� que el
   fallback es c�digo muerto en la pr�ctica.

2. **`TenantlessPrismaRepository` no satisface `Repository<TDto>`
   estructuralmente**: su `findAll(): Promise<TDto[]>` choca con
   `findAll(tenantId: string, ...)` del port, forzando `as unknown as XRepository`
   en `roles`/`tenants`/`audit`.

## Objetivo

Hacer `findPage` obligatorio en el port (eliminando el fallback de todos los
sitios) y alinear la firma de `TenantlessPrismaRepository` con `Repository<TDto>`
para eliminar los casts.

## Tareas

- [x] Cambiar `findPage?` ? `findPage` (obligatorio) en `Repository`.
- [x] Alinear `TenantlessPrismaRepository.findAll` a `findAll(tenantId?: string,
      query?: QueryOptions)` (ignorando `tenantId`) y `findById(id, tenantId?)`
      para satisfacer el port sin casts.
- [x] Eliminar el fallback de paginaci�n de `CrudService.findPage` (queda un
      simple `return this.repository.findPage(...)`).
- [x] Eliminar el fallback del bloque `ListX` en los handlers thin y en
      `list-settings.handler.ts` (usar `findPage` directo).
- [x] Quitar `as unknown as XRepository` en `roles.module.ts`,
      `tenants.module.ts`, `audit.module.ts`.
- [x] `pnpm nx test base-backend` verde.

## Criterio de aceptaci�n

- `findPage` es obligatorio; no queda ning�n fallback `findAll ? meta` en el
  c�digo.
- `TenantlessPrismaRepository` implementa `Repository<TDto>` sin `as unknown as`.
- Typecheck + lint + tests verdes en `base-backend`.

## Notas / riesgos

- Revisar productos consumidores: si alg�n repo custom en `crm-*`/`josanz-*`
  implementaba el port SIN `findPage`, dejar� de compilar. Buscar
  `implements .*Repository` fuera de base y a�adir `findPage` o extender
  `PrismaRepository`.
