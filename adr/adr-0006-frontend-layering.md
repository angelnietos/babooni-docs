<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0006: Frontend layering & Angular/React parity</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted
- Date: 2026-07-09

## Context

The frontend foundation (`@base/frontend/*`) is the starting point for every
developer, but it had drifted: each feature re-implemented headers, confirm
dialogs, empty states, search and pagination; React features were flat `index.tsx`
files with no `layout/pages/components` parity to Angular; and the data-layer
surface diverged (`rbac` only in Angular, `data-table` vs `table` naming). There
was also no automated guard to stop UI primitives from accruing business logic or
features from importing the wrong layer.

The backend hexagonal core is **closed** and out of scope; this ADR only governs
the frontend and its cross-cutting governance.

## Decision

1. **`shared/` presentational module** in every `*-features` lib. Twelve
   cross-domain dumb components (`PageHeader`, `ConfirmDialog`, `EmptyState`,
   `SearchBar`, `TableToolbar`, `Pagination`, `StatusBadge`, `FilterBar`,
   `DetailDrawer`, `Breadcrumbs`, `LoadingState`, `ErrorState`) live only in
   `shared/`, are standalone + `OnPush` (Angular) / pure function components
   (React), and **never import the data-layer** (`@base/angular`, `@base/react`,
   product equivalents). `StatusBadge` maps state→tone via semantic tokens, not
   hardcoded colors.

2. **Feature folder shape** (`layout/ pages/ components/ <feature>.routes.{ts,tsx}
   index.ts`). Angular already conforms; React features are migrated to the same
   shape (Fase 1.1). `components/` may only compose `@base/*-ui` (or the
   product `@josanz/*-ui`) — never define raw HTML/CSS components.

3. **Angular ↔ React parity.** Same 12 `shared` components, same data-layer
   surface (`rbac` in both, `Table` naming unified). The `@arquetipos/*-features`
   libs are **thin barrels** that re-export `@base/*-features` (no local
   implementation); `@josanz/*-features` extend the base with product domains
   (e.g. `dashboard`, `billing`, `inventory`) but never import `@arquetipos/*`.

4. **Governance enforced in CI.** ESLint `@nx/enforce-module-boundaries` keeps the
   `layer:*` and **`runtime:*`** tags (`runtime:isomorphic|backend|angular|react`).
   Legacy `scope:*` tags were purged (F58); `check-lib-layout --strict` fails if they
   reappear. `runtime:*` is authoritative together with `layer:*`.
   `tools/checks/check-frontend-conventions.mjs` (CI job `verify` → `Frontend
   conventions`) validates the feature folder shape and the `shared` purity, and
   fails barrels that implement locally instead of re-exporting.
   `CONTRIBUTING.md`, `CODEOWNERS` and `.editorconfig` document the rules.

> **Tag note (post-F58):** Do not add `scope:angular`, `scope:browser-*`, or
> `scope:isomorphic`. Use `runtime:*` + `layer:*` only.

## Consequences

- Features stop duplicating cross-domain UI; visual consistency is centralized.
- A structural regression (missing `layout/`, business logic in `shared/`,
  duplication inside a template barrel) breaks CI instead of shipping.
- Domain libs follow `api → data-access → features ← ui` with `shell` lazy-loading
  features (Angular **and** React / Next / Ionic / RN). Arquetipos domains are
  thin re-exports of `@base/*` by default ([arquetipos-thin-libs.md](../frontend/arquetipos-thin-libs.md)).
- Gate: `pnpm check:frontend-conventions` (+ layout / native-first checks).

### Amendment (2026-07-24)

Fase 1.1 / “React flat + lenient” y el barrel monolítico
`@arquetipos/react-features` quedaron **superados**: existen
`@arquetipos/react-{domain}-features` (y base equivalentes) con
`layout/pages/components`. No tratar el texto histórico de “pending Fase 1.1”
como estado actual.

## See also

- Convention linter: `tools/checks/check-frontend-conventions.mjs`.
- Contributor guide: `CONTRIBUTING.md`.
- Shared components: `libs/base/frontend/{angular,react}/features/src/shared/`.
- UI SoT (Lit) & freeze: [ADR 0010](adr-0010-native-ui-lit-sot.md),
  [ui-strategy.md](../frontend/ui-strategy.md).
- Storybook: [ADR 0011](adr-0011-storybook-native-ui-first.md).
- Back to the [docs hub](../README.md).
