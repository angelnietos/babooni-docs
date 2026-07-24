# F80 dead atom inventory (F80-C1)

**Date:** 2026-07-24  
**Scope:** `@base/next-ui`, `@base/ionic-ui`, `@base/react-native-ui`, `@arquetipos/arquetipos-{angular,react}-ui`, `@base/{angular-ui,react-ui}` exports vs usage in `apps/arquetipos`, `apps/clientes`, `apps/productos-saas`, and their feature libs.

**Legend:** **KEEP** — SoT Lit atom, matrix-documented adapter, or brand re-export surface (Storybook / override point). **DELETE?** — zero refs outside owning package and not in [native-ui-adapter-matrix.md](./native-ui-adapter-matrix.md) as adapter/SoT.

## Summary

| Action | Count | Notes |
|--------|-------|-------|
| **KEEP (adapters)** | All SoT rows in matrix for Next/Ionic/RN | Storybook parity completed F80-D1 |
| **KEEP (brand native wrappers)** | `ArqNative*` in arquetipos UI | Used in native-ui demo pages + some Angular panels |
| **DELETE executed** | 0 | No zero-ref duplicates found after F79 layout move |
| **DELETE deferred** | 0 | Conservative pass — inventory only |

## Adapter packages (`@base/next-ui`, `@base/ionic-ui`, `@base/react-native-ui`)

Per [native-ui-adapter-matrix.md](./native-ui-adapter-matrix.md). These are **KEEP** even when apps do not import them yet (Next/Ionic/RN template apps are thin; atoms are documented in Storybook).

| Package | SoT atoms exported | App/feature usage | Disposition |
|---------|-------------------|-------------------|-------------|
| `@base/next-ui` | Alert, Badge, Button, Card, Input, Select, Icon, EmptyState, LoadingState (+ composites) | `apps/arquetipos/frontend/nextjs/*` path alias only; features not wired to all atoms | **KEEP** — adapter SoT |
| `@base/ionic-ui` | Button, Input, Card, Badge, Avatar, EmptyState, Skeleton, Segment (+ chrome) | No direct imports in clientes/productos-saas | **KEEP** — adapter SoT |
| `@base/react-native-ui` | Full atom set + chrome | No app imports in clientes/productos-saas | **KEEP** — adapter SoT |

**Composites** (`ArqPageHeader`, `ArqTable`, `ArqMobileShell`, Ion chrome, etc.): **KEEP** — platform chrome class in matrix; optional Storybook later.

## Brand thin UI (`@arquetipos/arquetipos-angular-ui` / `@arquetipos/arquetipos-react-ui`)

Features import the **brand barrel**, not `@base/*` directly (enforced by ownership checks).

| Export class | Example symbols | Usage in arquetipos features | Disposition |
|--------------|-----------------|------------------------------|-------------|
| Base chrome re-exports | `LayoutShell`, `Topbar`, `PageHeader`, … | App shells / layouts | **KEEP** |
| Lit adapters (canonical) | `ArqNativeButton`, `ArqNativeInput`, `ArqNativeBadge`, … | All W1–W3 features + demos (F81-C1) | **KEEP** |
| CSS dual / F19 premium | `ArqButton`, `ArqBadge`, `ArqSpinner`, … | — | **DELETE (F81-D1)** — retired; Storybook → `ArqNative*` |

**F81-D1:** one canonical Button/Input/Alert/Badge/Spinner/… per runtime = `ArqNative*`.
Legacy CSS atoms and their stories removed from `atoms/{name}/`.


## `@base/angular-ui` / `@base/react-ui`

Kernel primitives consumed via brand packages. Unused-by-arquetipos kernel exports (e.g. some `Native*` not re-exported by arquetipos) remain **KEEP** for Josanz/SaaS/base apps.

## F80-D1 deletions considered

| Candidate | Ref check | Result |
|-----------|-----------|--------|
| Legacy `libs/base/frontend/next/ui/src/lib/Arq*.tsx` flat files | Index imports `atoms/` + `composites/` only; flat duplicates absent on disk | Already removed in F79 — nothing to delete |
| Legacy `libs/arquetipos/frontend/react/ui/src/lib/components` fork | Path does not exist | N/A |
| Duplicate Ionic/RN root `src/lib/Arq*.ts(x)` | Index points at `atoms/` / `chrome/` | Already consolidated — nothing to delete |

## Arquetipos visual sameness (Part D)

**Audit (grep):** No dedicated local Button/Input **CSS/SCSS forks** under `libs/arquetipos/frontend/*/features`. Feature components import `@arquetipos/arquetipos-{angular,react}-ui` (`ArqButton`, `ArqInput`, `ArqNativeButton`, `Input`, etc.) or use raw `<button>` only for **non-atom** controls (theme picker swatches, table row actions) — not parallel design systems.

**Conclusion:** Template domains stay visually aligned by consuming brand UI that re-exports or thin-wraps `@base/*` **Lit adapters** (`ArqNative*`) and kernel primitives; do not add feature-level atom CSS.

See also: [arquetipos-thin-libs.md](./arquetipos-thin-libs.md) (UI atoms → Lit adapters).

## CI note (F80-D1)

`.github/workflows/ci.yml` Storybook `run-many` now includes `base-next-ui`, `base-ionic-ui`, `base-react-native-ui`. `@base/ui-tokens` workspace deps added to `@base/next-ui` and `@base/ionic-ui`; Ionic tokens loaded via `project.json` `styles` (not TS import) for Angular builder compatibility.
