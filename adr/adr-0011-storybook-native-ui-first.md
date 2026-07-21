# ADR 0011: Storybook — native-ui first + Nx `storybook` serve

- Status: accepted
- Date: 2026-07-22

## Context

Storybook is the visual contract for UI packages, but tooling was uneven:

| Project | `build-storybook` | `storybook` (serve) |
|---------|-------------------|---------------------|
| `josanz-angular-ui` | yes | yes |
| `base-angular-ui` / `base-react-ui` | yes | **no** |
| `base-native-ui` | **no** | **no** |
| arquetipos `*-ui` | configs orphaned | no targets |

The Lit SoT (`@base/native-ui`) had **zero** stories, so designers/devs had to
boot Angular or React Storybooks to see shared atoms. That reinforces
framework-centric work and contradicts ADR 0010.

CI only built `base-angular-ui` and `base-react-ui` Storybooks.

Plans: F53-B1/B2/D2, F54-B1.

## Decision

1. **Primary catalog Storybook** is **`base-native-ui`** (Lit / web-components):
   both **`storybook` (serve)** and **`build-storybook`**. Default local port
   **4400** (document in design-system; adjust if taken).

2. **Framework adapter Storybooks** (`base-angular-ui`, `base-react-ui`) keep
   documenting **wrappers and remaining legacy** components; each must expose
   Nx **`storybook` serve** in addition to `build-storybook` (ports e.g. 4402 /
   4403; Josanz stays on 4401).

3. **Ownership:** stories live in the package that **owns** the component.
   Product Storybooks (Josanz, Arquetipos) document brand wrappers only — do
   not re-story base atoms unless the wrapper changes the visual API.

4. **Arquetipos `.storybook` folders** must either gain Nx targets or be
   explicitly deprecated in favor of native + base adapter SBs (F53-D2).

5. **CI:** keep building base Angular/React SBs; add `base-native-ui`
   `build-storybook` (F54 may add visual regression / Chromatic).

6. **Root scripts** (optional but preferred): `pnpm storybook:native-ui`,
   `storybook:base-angular`, `storybook:base-react` → `nx storybook …`.

## Consequences

- Visual SoT is reviewable without an Angular/React host app.
- More CI time when native SB is added — mitigate with Nx cache / nightly.
- Until F53-B* lands, commands above are **aspirational**; this ADR is the
  contract implementers must meet.
- Chromatic/Loki remain optional (F54-B1); not required for acceptance of 0011.

## See also

- ADR [0010](adr-0010-native-ui-lit-sot.md)
- [design-system.md](../frontend/design-system.md) § Storybook
- [ui-strategy.md](../frontend/ui-strategy.md) § Storybook
- Reference serve target: `josanz-angular-ui` `project.json`
