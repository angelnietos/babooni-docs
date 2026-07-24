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

Piloto F65: **clients**. Extensión F66-D1: **users / roles / audit** (y settings
si aplica) con la **misma** forma de puerto.

## Matriz canónica (stack → herramienta)

| Stack | Herramienta detrás del puerto | API del puerto |
|-------|-------------------------------|----------------|
| Angular SPA | `EntityListStore` / service + `{Domain}Facade` alias | signals `items` / `loading` / `error` + `load` / `create` / `update` / `remove` (audit: load only) |
| React SPA | **react-query** (`use{Domain}Facade`) | `items` / `loading` / `error` + mutations según dominio |
| Ionic | `Ionic{Domain}Facade` (signals + in-memory) | same shape |
| Next | `use{Domain}Facade` (hook owns list) | same shape |
| RN | `use{Domain}Facade` (hook owns list) | same shape |

### Multi-dominio (F66-D1)

| Dominio | Angular | React | Ionic |
|---------|---------|-------|-------|
| clients | `ClientsFacade` | `useClientsFacade` | `IonicClientsFacade` |
| users | `UsersFacade` | `useUsersFacade` | `IonicUsersFacade` |
| roles | `RolesFacade` | `useRolesFacade` | `IonicRolesFacade` |
| audit | `AuditFacade` (read) | `useAuditFacade` | `IonicAuditFacade` |

Legacy dual stores (`createFeatureState` / RTK list stores) remain for auth/session and
legacy app registration; **list CRUD panels** must use the facade. ESLint/check:
`pnpm check:features-no-store` (+ `:strict`).

**F67-B3:** `STRICT_DOMAINS` incluye **clients / users / roles / audit** —
`pnpm check:features-no-store:strict` verde. Carry closed.

**Related:** panel SoC
([features-layout-soc.md](./features-layout-soc.md)), entity field views
([entity-view-abstractions.md](./entity-view-abstractions.md)), FeatureShell
([feature-shell-presentation.md](./feature-shell-presentation.md)). Facade
multi-dominio y ratchet features↔store (F66-D1 / F66-B3 / F67-B3, cerradas).

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

- Origen: F65-D1 (cerrada, solo en git)
- [ADR 0006](../adr/adr-0006-frontend-layering.md) · F41 state strategy
- Guías: [add-frontend-domain.md](../guides/add-frontend-domain.md),
  [add-mobile-domain.md](../guides/add-mobile-domain.md)
