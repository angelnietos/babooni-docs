<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Front state + SoC — facade / port (F65-D1)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Features must depend on a **data-access port (facade)**, not on a concrete store
tool (NgRx `Store`, RTK `useAppDispatch`, mutable demo arrays). Goal: swap the
implementation behind the port and unit-test panels with a mock.

Piloto: **clients** (Angular, React, Ionic, Next, RN).

## Matriz canónica (stack → herramienta)

| Stack | Herramienta detrás del puerto | API del puerto |
|-------|-------------------------------|----------------|
| Angular SPA | `EntityListStore` (`ClientsService` / `ClientsFacade`) | signals `items` / `loading` / `error` + `load` / `create` / `update` / `remove` |
| React SPA | **react-query** (`useClientsFacade`) | `items` / `loading` / `error` + `load` / `create` / `remove` |
| Ionic | `IonicClientsFacade` (signals + in-memory) | same shape |
| Next | `useClientsFacade` (hook owns list) | same shape |
| RN | `useClientsFacade` (hook owns list) | same shape |

Legacy dual stores (`createFeatureState` / RTK `createEntityListStore` for list
CRUD) were removed from the clients piloto — apps register auth/users/roles/audit
stores only; clients CRUD goes through the facade.

**F66 follow-ups:** panel SoC
([features-layout-soc.md](./features-layout-soc.md)), entity views
([entity-view-abstractions.md](./entity-view-abstractions.md)), ESLint
features↔store ([F66-B3](../plans/rounds/plans-66-sixty-six-round/1750000234000-f66-eslint-features-store-ratchet.md)).

## Reglas SoC

| Capa | Vive aquí | No vive aquí |
|------|-----------|--------------|
| `api` | DTOs / contracts | HTTP, UI |
| `data-access` | Facade/port + tool wiring | Templates, toast/confirm UX |
| `features` | Compose UI + call facade; local UI state (query, form, confirm) | `@ngrx/store`, `useAppDispatch`, `DEMO_*` mutable arrays, RTK list stores |
| `ui` | Presentational | Domain fetch |

Auth session may still use its own store/service; **clients** CRUD must not.

## Mock en Jest

**Angular**

```ts
const facadeMock = {
  items: signal([]),
  loading: signal(false),
  error: signal(null),
  load: jest.fn(),
  create: jest.fn(),
  remove: jest.fn(),
};
TestBed.configureTestingModule({
  providers: [{ provide: ClientsFacade, useValue: facadeMock }],
});
```

**React**

```ts
jest.mock('@base/react-clients-data-access', () => ({
  useClientsFacade: () => ({
    items: [],
    loading: false,
    error: null,
    load: jest.fn(),
    create: jest.fn(),
    remove: jest.fn(),
  }),
}));
```

**Ionic / Next / RN** — mock the facade class or `useClientsFacade` the same way;
do not import NgRx/RTK TestBed for list CRUD.

## Enlaces

- Plan: [F65-D1](../plans/rounds/plans-65-sixty-five-round/1750000228000-f65-frontend-state-soc.md)
- [ADR 0006](../adr/adr-0006-frontend-layering.md) · F41 state strategy
- Guías: [add-frontend-domain.md](../guides/add-frontend-domain.md),
  [add-mobile-domain.md](../guides/add-mobile-domain.md)
