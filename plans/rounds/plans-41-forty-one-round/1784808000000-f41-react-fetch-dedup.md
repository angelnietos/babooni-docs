<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-F2 � Deduplicar fetch/query en React</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Media  
**Estado:** Completado
**Dependencias:** F41-F1 (la fuente elegida define d�nde vive el helper)

## Contexto

Los hooks de listado React son copia literal (solo cambian path + tipo):

- `react/domains/clients/data-access/src/use-clients.ts`
- `react/domains/users/data-access/src/use-users.ts`
- `react/domains/roles/data-access/src/use-roles.ts`
- `react/platform/kernel/lib/projects/use-projects.ts`
- `react/platform/kernel/lib/inventory/use-inventory.ts`
- `react/platform/kernel/lib/billing/use-billing.ts`

Patr�n repetido:

```typescript
async function fetchX(): Promise<XDto[]> {
  const res = await authorizedFetch('/api/x');
  if (!res.ok) throw new Error('Failed to load x');
  return (await res.json()) as XDto[];
}
export function useX() { return useQuery({ queryKey: ['x'], queryFn: fetchX }); }
```

Adem�s, el parseo `Array.isArray(json) ? json : json.data` y el POST/DELETE con
`authorizedFetch` est�n duplicados entre `create-entity-list-store.ts` y los
stores de dominio (`users.store.ts`, `roles.store.ts`).

## Objetivo

Extraer helpers compartidos para el hook de listado y para el request JSON, de
modo que cada dominio quede en 1-2 l�neas.

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

## Criterio de aceptaci�n

- Cero copias del bloque `fetchX + useQuery`.
- `parseListJson`/`jsonRequest` en un �nico lugar.
- Typecheck + lint + tests verdes.

## Notas / riesgos

- `authorizedFetch` viene de `@base/react-auth-data-access`; mantener esa
  dependencia en el helper compartido, no en cada dominio.
- Respetar los `queryKey` existentes para no invalidar cach�s de features.
