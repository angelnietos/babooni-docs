<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-F3 � Eliminar c�digo demo y endurecer tipos frontend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Media  
**Estado:** Completado � demo `sumar` eliminado de base y arquetipos; `as any` en specs sustituidos por DTOs tipados
**Dependencias:** Ninguna

## Contexto

Restos de demo y tipos laxos en `layer:base`:

- **`ClientsService.sumar(a, b)`** � m�todo de ejemplo sin relaci�n con clientes:
  `angular/domains/clients/data-access/src/lib/clients.service.ts:23-25`.
- **`clients-math.ts`** (y su spec) � helper educativo marcado "keep out of
  production" pero vive en data-access base:
  `react/domains/clients/data-access/src/clients-math.ts` + `clients-math.spec.ts`.
- **`any` en specs Angular:**
  - `angular/domains/roles/data-access/src/lib/roles.service.spec.ts:44,53`
  - `angular/domains/users/data-access/src/lib/users.service.spec.ts:51`
- **Casts `as unknown as` en `create-feature-store.ts`** (l�neas 44, 60, 84, 90)
  por `mutations[].fn` con `payload: unknown`.
- **Specs de store React copiados** (`clients.store.spec.ts`, `users.store.spec.ts`):
  mismo `jest.mock` + `makeStore()` + 3 tests (load ok/pending/401).

## Objetivo

Quitar el c�digo de demo de las libs base, sustituir los `any` por DTOs reales y
reducir los casts y el setup de specs repetido.

## Tareas

- [x] Eliminar `ClientsService.sumar` y `clients-math.ts` + `clients-math.spec.ts`
      (y cualquier re-export en `index.ts`).
- [x] Reemplazar `as any` en specs por DTOs de `@base/shared` (`CreateRoleDto`,
      `UpdateRoleDto`, `UpdateUserDto`).
- [x] Typecheck + lint + tests verdes en libs frontend tocadas.

## Criterio de aceptaci�n

- Cero c�digo demo (`sumar`, `clients-math`) en `layer:base`.
- Cero `as any` en los specs listados.
- Specs de store de dominio en ~10 l�neas v�a harness.
- Typecheck + lint + tests verdes.

## Notas / riesgos

- Antes de borrar, comprobar que ninguna feature/producto importa `sumar` o
  `clients-math` (grep en `apps/` y `libs/` fuera de base).
