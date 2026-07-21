# ADR 0001: Hexagonal architecture

- Status: accepted
- Date: 2026-07-08

## Context

The platform ships reusable reference implementations (`@arquetipos/*`) and
final customer products (`@josanz/*`) on top of a shared kernel (`@base/*`).
They must share business logic without leaking framework or database concerns,
and new domains must be added cheaply and consistently.

## Decision

Adopt a hexagonal (ports & adapters) layout per domain:

```
domains/<x>/
  domain/              # entity, errors, mappers, value-objects
  ports/               # repository port (and other driven ports)
  application/         # Nest CQRS commands/queries/handlers + facade
  adapters/
    persistence/       # Prisma repository adapters
    http/              # Nest controllers (+ Nest companions)
  <x>.module.ts        # composition root
shared/
  domain|application|adapters|cqrs|di/
crosscutting/          # auth, messaging, sagas, http helpers
integration/           # int-spec scaffolds
```

The application layer depends only on ports (`Repository`, `DomainEventBus`).
Adapters (Nest, Prisma) implement those ports. The shared kernel never
imports `@arquetipos/*` or product code; products import `@base/*` only and add
their own thin app shell (`apps/`).

## Consequences

- New domains follow one template → low variance, easy review.
- Framework swaps (Nest ↔ other) are localized to `adapters/`.
- A domain convention linter (`tools/scripts/check-domain-conventions.mjs`)
  enforces the structure automatically (see ADR-adjacent tooling).
- Slightly more boilerplate than a single fat service, accepted as the cost of
  reuse.

> **Update (F33, 2026-07-17):** the application layer now uses a single
> **Nest CQRS** pattern (commands/queries/handlers + a thin bus-dispatching
> facade). The original "use-cases + service" and generic `CrudService` variants
> are deprecated. See [adr-0009-cqrs-nest.md](adr-0009-cqrs-nest.md).

> **Update (2026-07-18):** dropped the `hex/` umbrella — `domains/`,
> `crosscutting/`, `shared/`, and `integration/` sit beside `platform/` under
> `libs/base/backend/src/lib/`. Per domain, repository ports moved to `ports/`
> and infrastructure split into `adapters/{persistence,http}/` with
> `{domain}.module.ts` at the slice root. `shared/infrastructure/` renamed to
> `shared/adapters/`.

## See also

- Scaffolding how-to + domain template: `plans/README.md` §7.
- Convention linter: `tools/scripts/check-domain-conventions.mjs`.
- New-domain generator: `tools/scripts/new-domain.mjs`.
- Service catalog: `SERVICES.md`.
- Back to the [docs hub](../README.md).
