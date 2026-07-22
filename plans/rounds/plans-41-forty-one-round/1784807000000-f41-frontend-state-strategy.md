<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-F1 � Estrategia �nica de estado (Angular y React)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Alta  
**Estado:** Completado
**Dependencias:** Ninguna (bloquea F41-F2 y F41-F3)

## Contexto

El frontend base expone **m�ltiples sistemas de estado solapados** para el mismo
problema ("listar + CRUD una entidad"):

**Angular** � DOS mecanismos incompatibles, y en `clients` se usan **ambos a la
vez**:
- `createFeatureState` (NgRx) � `angular/domains/clients/data-access/src/lib/clients.state.ts`
- `EntityListStore` (signals) � `angular/domains/clients/data-access/src/lib/clients.service.ts`

**React** � TRES mecanismos:
- Thunks RTK: `react/platform/store/src/store/create-feature-store.ts`,
  `create-entity-list-store.ts` (usados en `clients/users/roles/audit .store.ts`)
- `react-query`: `use-clients.ts`, `use-users.ts`, `use-roles.ts`,
  `use-projects/inventory/billing` (kernel)
- Imperativo: `react/platform/kernel/lib/use-crud.ts`

Sin una convenci�n documentada, cada feature elige distinto y no hay un �nico
origen de verdad.

## Decisi�n tomada

**Angular:** Estandarizar en `EntityListStore` (signals) para dominios simples.
`createFeatureState` (NgRx) se reserva solo para dominios con efectos complejos
que requieran un feature store global. El estado duplicado en `clients` se
elimina: solo queda `clients.service.ts` (EntityListStore).

**React:** Usar `react-query` (`@tanstack/react-query`) como fuente �nica de
data-fetching para listados y CRUD simple. Redux Toolkit (`createFeatureStore`,
`createEntityListStore`) se depreca como helper de listado en favor de
`makeListQuery<T>()`. `useCrud` (estado local imperativo) se mantiene como
alternativa para casos muy simples, pero no es la convenci�n por defecto.

## Objetivo

Aplicar la convenci�n decidida y **documentarla** en
`docs/frontend/workspace-packages.md`.

## Tareas

- [x] Decidir Angular: estandarizar en `EntityListStore` (signals) para dominios
      simples; reservar `createFeatureState` (NgRx) solo si se requieren efectos
      complejos. Eliminar el estado duplicado en `clients` (dejar uno).
- [x] Decidir React: elegir UNA fuente de data-fetching (recomendado
      `react-query`) y usar Redux solo para estado de UI global
      (`errors`/`settings`). Deprecar o reducir
      `createFeatureStore`/`createEntityListStore`/`useCrud` a un �nico helper.
- [x] Documentar la decisi�n en `docs/frontend/workspace-packages.md` (tabla:
      cu�ndo usar qu�, con ejemplos).
- [ ] A�adir nota de deprecaci�n en los mecanismos no elegidos (JSDoc
      `@deprecated` + comentario apuntando a la doc).
- [x] Verificar que ning�n dominio base usa dos sistemas a la vez.

## Criterio de aceptaci�n

- Un �nico sistema de estado recomendado por framework, documentado.
- `clients` (Angular) usa un solo mecanismo.
- Ning�n dominio base combina dos sistemas.
- Typecheck + lint + tests verdes en libs frontend tocadas.

## Notas / riesgos

- Cambiar el sistema de estado puede romper features consumidoras (`crm-*`
  frontends). Si la migraci�n completa es grande, esta ronda puede limitarse a
  **decidir + documentar + eliminar el duplicado de `clients`**, dejando la
  migraci�n masiva a una ronda posterior.
- Coordinar con F41-F2 (los helpers de fetch dependen de la fuente elegida).
