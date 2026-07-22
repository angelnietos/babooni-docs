<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0009: Nest CQRS command/query separation in the hexagonal kernel</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted (F33)
- Date: 2026-07-17
- Supersedes part of: [adr-0001-hexagonal-architecture.md](adr-0001-hexagonal-architecture.md)
- Formerly: `adr-00xx-cqrs-nest.md`

## Context

ADR-0001 fixed a hexagonal layout but left the *application layer* open: some
domains used plain `use-case` classes (`clients`), one used Nest
`CommandBus`/`QueryBus` (`audit`), and the rest used a generic `CrudService`
over a Prisma delegate. That variance makes an AI restriction strategy
impossible to apply uniformly — we cannot intercept "all writes" when writes
are scattered across services.

F33 requires one uniform pattern.

## Decision

Adopt **Nest CQRS** as the single application-layer pattern for every domain in
`@base/backend` (`libs/base/backend/src/lib/domains/`):

- Writes are `Command`s handled by `@CommandHandler`s.
- Reads are `Query`s handled by `@QueryHandler`s.
- The domain facade (`*Service`) only injects `CommandBus`/`QueryBus` and
  dispatches — it holds no business logic (legacy `use-case` classes and
  `CrudService` are deprecated).
- Handlers are auto-discovered by Nest's `CqrsModule`; no per-feature bus
  registration.
- The shared kernel (`CqrsInfraModule`) provides `CqrsModule`, `UnitOfWork`
  and the AI gateways.

This makes "every write goes through the `CommandBus`" a structural guarantee,
so the AI boundary is enforced by routing: `AiQueryGateway` holds a `QueryBus`
only; an AI `CommandBus` (future `AiCommandGateway`) is an explicit allow-list.

## Consequences

- `tools/checks/check-domain-conventions.mjs` enforces
  `application/<x>/{commands,queries,handlers}/` and forbids loose
  `infrastructure/*.module.ts` (use `--strict` in CI).
- `tools/scaffolds/new-domain.mjs` scaffolds the CQRS template directly.
- `audit` keeps its existing CQRS handlers under `application/`.
- `clients` is the reference template for handlers.
- More files per domain, accepted as the cost of a uniform, AI-gateable seam.

## See also

- AI policy: [ai-cqrs-policy.md](../guides/ai-cqrs-policy.md).
- Convention linter: `tools/checks/check-domain-conventions.mjs`.
- Testing handlers: [testing-pyramid.md](../guides/testing-pyramid.md).
- Back to the [docs hub](../README.md).
