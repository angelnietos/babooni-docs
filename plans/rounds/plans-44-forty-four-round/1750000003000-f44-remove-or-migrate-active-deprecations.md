# F44-A4 — Eliminar / migrar deprecated activos

## Estado

completado

## Objetivo

Eliminar o migrar los call sites que todavía usan APIs deprecadas, respetando el ciclo definido en F44-A2.

## Entradas

- Resultado de F44-A2 (política de deprecación).
- Tooling de F44-A3 (`check:deprecated`).
- Inventario de F44-A1.

## Tareas

1. **Ruta `GET /clients/by-email/:email`**:
   - Marcar para remoción en próxima major o mover a `/v1`.
   - Actualizar ejemplos y tests.
2. **Aliases `*Command` en `clients.dtos.ts`**:
   - Mover a módulo de compatibilidad separado si aún hay call sites externos.
   - Actualizar imports internos a DTOs nuevos.
3. **`TENANT_MODE` / `IS_MULTI_TENANT`**:
   - Reemplazar por `resolveTenantMode()` / `isMultiTenant()` en todo el repo.
   - Remover exports solo después de que no haya usos.
4. **Cualquier otro deprecated del inventario**:
   - Aplicar mismo ciclo: minor para nuevos deprecated, major para remoción.

## Restricción

No cambiar firmas públicas sin pasar por el ciclo de deprecación definido en F44-A2.

## Criterios de aceptación

- [ ] `pnpm check:deprecated` pasa sin errores.
- [ ] No hay call sites activos de deprecated sin marca `allowed`.
- [ ] Tests actualizados.

## Dependencias

- F44-A2
- F44-A3
