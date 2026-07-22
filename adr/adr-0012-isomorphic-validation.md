<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0012: Isomorphic validation (FE ↔ BE)</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted
- Date: 2026-07-22
- Plan: [F65-B1](../plans/rounds/plans-65-sixty-five-round/1750000225000-f65-carry-isomorphic-validation.md)

## Context

DTOs in `@base/shared` already use **class-validator** (Nest `ValidationPipe`).
Frontends re-implement `required` / `email` ad hoc. F59+ deferred a shared kit
while debating Zod vs class-validator as a second SoT.

## Decision

1. **Source of truth for HTTP / Nest:** keep **class-validator** decorators on
   DTOs in `@base/shared` (and Nest pipes). Do **not** introduce Zod (or another
   schema library) as a parallel SoT without a superseding ADR.
2. **Isomorphic sync rules for FE:** pure predicates in
   `@base/shared` (`lib/validation/`) that **mirror** DTO rules (clients pilot:
   `validateCreateClient` / `validateUpdateClient`). Adapters (Angular
   `Validators`, React messages) call these predicates — they do not own rules.
3. **Async rules** (unique email, …): **ports** on the backend + stable error
   codes for the FE. Not isomorphic.
4. **Zod / Standard Schema kit:** carry **[F67-B2](../plans/rounds/plans-67-sixty-seven-round/1750000244000-f67-carry-zod-kit.md)**
   if we need one schema language for non-Nest consumers; until then, no second
   library in app deps.

## Consequences

- FE forms can share clients Create/Update checks without Nest runtime.
- BE ↔ shared DTO drift (e.g. optional `tenantId` on Nest-only DTOs) remains a
  separate cleanup (F67 / shared DTO alignment).
- Products must not add Zod “just for forms” while this ADR stands.

## Links

- [isomorphic-validation-defer.md](../frontend/isomorphic-validation-defer.md)
- F59 strategy (historical):
  [plans-59](../plans/rounds/plans-59-fifty-nine-round/1750000160000-f59-isomorphic-validation-strategy.md)
