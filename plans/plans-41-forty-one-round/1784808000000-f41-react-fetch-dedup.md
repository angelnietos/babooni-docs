# F41-F2 — Deduplicar fetch/query en React

**Prioridad:** Media  
**Estado:** Completado
**Dependencias:** F41-F1 (la fuente elegida define dónde vive el helper)

## Contexto

Los hooks de listado React son copia literal (solo cambian path + tipo):

- `react/domains/clients/data-access/src/use-clients.ts`
- `react/domains/users/data-access/src/use-users.ts`
- `react/domains/roles/data-access/src/use-roles.ts`
- `react/platform/kernel/lib/projects/use-projects.ts`
- `react/platform/kernel/lib/inventory/use-inventory.ts`
- `react/platform/kernel/lib/billing/use-billing.ts`

Patrón repetido:

```typescript
async function fetchX(): Promise<XDto[]> {
  const res = await authorizedFetch('/api/x');
  if (!res.ok) throw new Error('Failed to load x');
  return (await res.json()) as XDto[];
}
export function useX() { return useQuery({ queryKey: ['x'], queryFn: fetchX }); }
```

Además, el parseo `Array.isArray(json) ? json : json.data` y el POST/DELETE con
`authorizedFetch` están duplicados entre `create-entity-list-store.ts` y los
stores de dominio (`users.store.ts`, `roles.store.ts`).

## Objetivo

Extraer helpers compartidos para el hook de listado y para el request JSON, de
modo que cada dominio quede en 1-2 líneas.

## Tareas

- [x] Crear `makeListQuery<T>(key, path)` (en el paquete de la fuente elegida en
      F41-F1) que devuelva el hook `useQuery`. Uso:
      `export const useClients = makeListQuery<ClientDto>('clients', '/api/clients');`
- [x] Crear `parseListJson<T>(res)` (maneja `Array.isArray ? json : json.data`) y
      `jsonRequest(path, init)` (headers JSON + `authorizedFetch` + manejo de
      error) compartidos.
- [x] Migrar los 6 hooks `useX` y los stores de dominio a los helpers.
- [x] Eliminar el fetch/parseo inline duplicado.
- [x] Tests verdes en libs React tocadas.

## Criterio de aceptación

- Cero copias del bloque `fetchX + useQuery`.
- `parseListJson`/`jsonRequest` en un único lugar.
- Typecheck + lint + tests verdes.

## Notas / riesgos

- `authorizedFetch` viene de `@base/react-auth-data-access`; mantener esa
  dependencia en el helper compartido, no en cada dominio.
- Respetar los `queryKey` existentes para no invalidar cachés de features.
