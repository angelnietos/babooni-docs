<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0011: Storybook — native-ui first + Nx `storybook` serve</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

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

1. **Primary catalog Storybook** is **`base-native-ui`** (Lit custom elements):
   both **`storybook` (serve)** and **`build-storybook`**. Framework:
   **`@storybook/web-components-vite`**. Stories are **`.ts`** using Lit
   `html\`...\`` — **not** React JSX / `@storybook/react-vite`. Default local
   port **6007** (document in design-system).

2. **Every framework adapter has its own Storybook** documenting **wrappers**
   of the Lit SoT (or token-aligned twins for RN). Coverage of the atom set
   must stay **parity-aligned** with `base-native-ui` — no stack left as a
   thinner catalog.

   | Package | Framework | Stories | Role |
   |---------|-----------|---------|------|
   | `base-native-ui` | `@storybook/web-components-vite` | `.ts` + Lit `html` | **SoT catalog** |
   | `base-angular-ui` | `@storybook/angular` | `.ts` | Angular CE wrappers |
   | `base-react-ui` | `@storybook/react-vite` | `.tsx` | React CE wrappers |
   | `base-next-ui` | `@storybook/react-vite` | `.tsx` | Next adapters (`'use client'` / CE islands) |
   | `base-ionic-ui` | `@storybook/angular` | `.ts` | Ionic adapters over Lit / Ion chrome |
   | `base-react-native-ui` | `@storybook/react-vite` (+ RN-web) | `.tsx` | Token/API twins (no Lit in RN bundle) |
   | Product UI (Josanz / Arquetipos) | Angular / React | brand only | Wrappers on top of base |

   Ports (local): native 6007 · Josanz 4401 · Angular 4402 · React 4403 ·
   Arquetipos Angular/React 4404/4405 · Next 4406 · Ionic 4407 · RN 4408.

3. **Ownership:** stories live in the package that **owns** the component.
   Product Storybooks document brand wrappers only — do not re-story base Lit
   atoms unless the wrapper changes the visual API.

4. **Arquetipos templates** must look the same across Angular / React / Next /
   Ionic / RN by consuming the **same Lit SoT** (or RN token twin), not
   parallel one-off atoms.

5. **CI:** build `base-native-ui` + adapter SBs (angular/react required;
   next/ionic/rn as targets land). F54+ may add Chromatic.

6. **Root scripts:** `pnpm storybook:native-ui`, `storybook:base-angular`,
   `storybook:base-react`, `storybook:base-next`, `storybook:base-ionic`,
   `storybook:base-rn`.

### Amendment (2026-07-24) — Lit SoT ≠ React host

Earlier scaffolding hosted Lit CEs inside `@storybook/react-vite` via a
`Ce` bridge. That contradicted ADR 0010.

**Superseded:** React-hosted native-ui stories (`.tsx` + `Ce`).
**Current:** `@storybook/web-components-vite` + Lit `html` stories (`.ts`).

### Amendment (2026-07-24) — adapter parity (Next / Ionic / RN)

Do **not** treat Next / Ionic / RN as “no Storybook”. Each adapter documents
the same atom set. Next loads Lit only in client islands (SSR-safe); RN keeps
token/API parity per ADR 0010 §6.

## Consequences

- Visual SoT is reviewable without an Angular/React host app.
- More CI time when native SB is added — mitigate with Nx cache / nightly.
- Until F53-B* lands, commands above are **aspirational**; this ADR is the
  contract implementers must meet.
- Chromatic/Loki remain optional (F54-B1); not required for acceptance of 0011.

## See also

- ADR [0010](adr-0010-native-ui-lit-sot.md)
- [design-system.md](../frontend/design-system.md) § Storybook
- [native-ui-adapter-matrix.md](../frontend/native-ui-adapter-matrix.md)
- [ui-strategy.md](../frontend/ui-strategy.md) § Storybook
- Reference serve target: `josanz-angular-ui` `project.json`
- Plan: [F79](../plans/rounds/plans-79-seventy-nine-round/) — adapter Storybook parity + Lit migration
