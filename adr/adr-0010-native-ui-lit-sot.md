<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0010: Lit `@base/native-ui` as cross-framework UI SoT</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted
- Date: 2026-07-22

## Context

The monorepo ships **many** front-end runtimes (Angular, React, Next, Ionic,
React Native). Without a shared primitive layer, each stack re-implements
Button / Input / Alert / … — N visual sources of truth, expensive redesigns,
and AI-generated one-off markup in features.

`@base/native-ui` already provides Lit **Custom Elements** (framework-agnostic
DOM). Framework packages (`@base/angular-ui`, `@base/react-ui`,
`@base/ionic-ui`, `@base/react-native-ui`) still contain **parallel**
framework-native primitives that duplicate the same atoms. Continuing to grow
those parallels defeats the Lit investment and multiplies maintenance.

React Native **cannot** host Lit CEs in the native tree; it needs a parallel
implementation aligned by **tokens / conceptual API**, not by the same runtime.

Product brand packages (`@josanz/angular-ui`, `@arquetipos/*-ui`,
`@saas/shared-ui`) remain the place for branding — they should wrap or re-export
base adapters, not invent a fourth Button.

Plans: [F53](../plans/rounds/plans-53-fifty-three-round/),
[F54](../plans/rounds/plans-54-fifty-four-round/).

## Decision

1. **Source of truth (web / hybrid DOM hosts):** `@base/native-ui` (Lit CE).
   New generic primitives are implemented there first (tests + Storybook).

2. **Framework adapters:** `@base/angular-ui` / `@base/react-ui` (and Next/Ionic
   hosts) expose **thin wrappers** (`Native*`) that bind props/events and
   register CEs — they **cap or extend** the CE API, they do not fork visuals.

3. **Freeze (from F53 onward):** Do **not** add or evolve feature-level
   framework-only primitives in `@base/*-ui` (new variants, redesigns) except
   critical bug/security fixes. Existing framework-only components may remain
   until migrated/deprecated (F54); they are **legacy parallel**, not the
   growth path.

4. **Apps & features** (arquetipos, clientes, SaaS) **prefer** native wrappers
   (or raw CE where the host is already CE-friendly, e.g. Ionic demos) over
   legacy framework primitives.

5. **Imports from other projects** land in `@base/native-ui` (or brand wrappers
   on top of CE), never as new framework-only islands in base.

6. **React Native:** keep `@base/react-native-ui`; align **design tokens and
   prop names** with native-ui (see F53-E1 / F54-B2). Do **not** ship Lit into
   the RN bundle.

7. **Product UI** (`@josanz/angular-ui`, etc.): wrap/re-export per
   [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md); branding
   stays product-owned. This ADR does **not** require deleting Josanz’s large
   brand catalog in one shot.

## Consequences

- One visual SoT for DOM-capable stacks; redesigns happen once in Lit.
- Short-term gap: not every CE has Angular/React wrappers yet (F53-A2).
- Legacy framework primitives create dual APIs until F54 deprecation/migration.
- RN remains a second implementation; token drift is the residual risk (mitigate
  with `@base/ui-tokens`).
- Governance: soft then strict “native-first” checks (F53-C2 / F54-C1).

## See also

- [ui-strategy.md](../frontend/ui-strategy.md)
- [design-system.md](../frontend/design-system.md)
- [how-it-works.md](../frontend/how-it-works.md) § UI
- ADR [0006](adr-0006-frontend-layering.md) (feature layering; complementary)
- ADR [0011](adr-0011-storybook-native-ui-first.md) (Storybook)
- Code: `libs/base/frontend/crosscutting/native-ui/`
