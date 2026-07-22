<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0008: Platform scope vs MVP client (opt-in stacks)</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted
- Date: 2026-07-18
- Tags: product, scaffolding, arquetipos

## Context

The monorepo ships **many** reference stacks: Angular and React web (single/multi-tenant),
Next.js, Ionic, React Native/Expo, Module Federation remotes, and backend shapes
(monolith, gateway, microservices). That breadth is valuable as a **platform catalog**,
but it is easy to misread as “every new client must stand up all of these on day one.”

New customer products (`apps/clientes/{slug}/`) historically struggled with onboarding
cost when teams copied the full arquetipos surface instead of a minimal viable slice.

## Decision

1. **Platform catalog (keep in-repo):** Arquetipos templates under `apps/arquetipos/` and
   matching `@arquetipos/*` / `@base/*` libs remain the **reference implementations**.
   They are not deleted; they demonstrate patterns and act as copy-paste sources.

2. **Default MVP for a new client product:**
   - **One** frontend: Angular **or** React (web SPA), single-tenant or multi-tenant as required
   - **One** backend composition root: Nest monolith on `@base/backend` (+ product `@…/backend`)
   - Shared DTOs in `@…/shared`
   - Auth via the existing Keycloak/JWT path already documented in ADR 0005

3. **Opt-in stacks** (add only when the customer contract requires them):
   - Next.js (marketing / SSR / SEO surfaces)
   - Ionic / React Native (mobile)
   - Module Federation remotes
   - Extra microservices beyond the monolith

4. **Scaffolding:** `scaffold-cliente-product` / checklists must default to the MVP slice
   and treat mobile/Next/MF/MS as **explicit flags**, not silent includes.

## Consequences

- Faster first green deploy for new clientes; less Windows MAX_PATH / graph pressure on day 1
- Platform still invests in Next/mobile/MF as **optional** templates (see F36 DX work)
- Product owners must explicitly choose opt-in stacks; “symmetry with arquetipos apps” is
  not a reason to enable them
- Docs (`nuevo-cliente-checklist`, guides) should link this ADR as the scope gate

## Alternatives considered

- **Always scaffold everything:** rejected — high cost, regressions (F8/F36 nested nm, path depth)
- **Delete unused stacks:** rejected — loses reference value; prefer opt-in documentation instead
