# UI lib folder layout (F80 → F81)

Contract for framework UI packages (`@base/*-ui`, brand UIs). Complements
[native-ui-adapter-matrix.md](./native-ui-adapter-matrix.md).

## Required tree

```text
{ui-lib}/src/
├── index.ts              # public barrel (stable npm API)
└── lib/
    ├── atoms/{name}/       # SoT adapters (Native* / ArqNative*) + RN twins
    ├── chrome/{name}/      # platform-only (Ionic tabs, shells…)
    ├── composites/{name}/  # layout shells, tables, page chrome, panels…
    ├── re-exports/         # optional — thin re-exports only
    └── theme/              # tokens + global CSS/SCSS
```

**Do not** place component sources as loose `.ts`/`.tsx` under `src/lib/`.
Global styles may live under `theme/` (see `@josanz/angular-ui`).

### Adapters = atoms (F81)

Lit CE adapters (`Native*` / `ArqNative*`) live in **`atoms/{name}/`** next to
(or replacing) any CSS dual. Confirm-dialog and similar → `composites/{name}/`.

**Prohibited (F81-B1+):** new files under `src/lib/wrappers/native/` (flat dump).
That folder is removed or left empty + deprecated after the split.

## Enforcement (F80-A1)

| Mode | Command |
|------|---------|
| Soft (default, CI) | `pnpm check:ui-lib-layout` |
| Strict (local / future CI) | `pnpm check:ui-lib-layout:strict` |

Script: `tools/checks/check-ui-lib-layout.mjs`. Allowlist:
`tools/checks/ui-lib-layout-allowlist.json` (`paths` / `packages`).

Seed packages must pass **without** allowlist: `@base/next-ui`, `@base/ionic-ui`,
`@base/react-native-ui`, `@base/native-ui`.

## Base kernel (F80-B1)

| Package | Status |
|---------|--------|
| `@base/next-ui` | `atoms/` + `composites/` |
| `@base/ionic-ui` | `atoms/` + `chrome/` + `theme/` |
| `@base/react-native-ui` | `atoms/` + `chrome/` + `composites/` + `theme/` |
| `@base/native-ui` | already `lib/{atom}/` (SoT) |
| `@base/angular-ui` / `@base/react-ui` | `atoms/{name}/` (Native* after F81-B1; was `wrappers/native`) |

## Brand packages (F80-B2 → F81-B1)

| Package | Layout |
|---------|--------|
| `@arquetipos/arquetipos-angular-ui` | `atoms/` + `composites/` + `theme/` (adapters in `atoms/{name}/`) |
| `@arquetipos/arquetipos-react-ui` | same |
| `@arquetipos/*-ui` (next / ionic / rn) | **thin barrels** — `src/index.ts` only until local components exist |
| `@saas/shared-ui` | `atoms/` + `composites/` |
| `@josanz/angular-ui` | **product taxonomy** (`forms/`, `layout/`, `actions/`, …) — see below |

Native CE adapters are atoms: one folder per component, same contract as seed
`@base/{next,ionic,react-native}-ui`.

## Brand packages ↔ apps

| Apps group | Brand UI |
|------------|----------|
| `apps/arquetipos` | `@arquetipos/*-ui` |
| `apps/clientes/{slug}` | `@josanz/angular-ui` (etc.) |
| `apps/productos-saas` | `@saas/shared-ui` |

## `@josanz/angular-ui` (F80 allowlist → F81-E1 / F82)

Large ERP catalog: folders like `forms/`, `feedback/`, `layout/`, `navigation/`,
`data-display/`, `overlays/`, `settings/`, `catalog/`, … map **conceptually** to
`atoms/` (single controls, badges, toasts) vs `composites/` (lists, shells, panels).

F80-B2 did **not** rename every product folder (import churn). Changes in F80:

- Global `styles.scss` / `styles.css` → `src/lib/theme/` (apps/Storybook paths updated).
- No loose `.ts` under `src/lib/` root (only folder taxonomy + `theme/`).

Full physical `atoms|composites` migration for Josanz is **deferred to F82**
(F81-E1 explícito — no bloquea cierre F81).

## Public API

Keep stable package barrels (`src/index.ts`). Internal moves must not break
named exports on `@arquetipos/*-ui`, `@saas/shared-ui`, or `@josanz/angular-ui`.
