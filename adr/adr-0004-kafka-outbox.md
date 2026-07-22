<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">ADR 0004: Kafka + transactional Outbox</h1>

<p align="center">
  <img alt="ADR" src="https://img.shields.io/badge/ADR-0d5f59?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

- Status: accepted
- Date: 2026-07-08

## Context

Domain events (client created, invoice approved, …) must reach other services
via Kafka, but a broker outage or a crash between the DB write and the publish
must not lose events. A naive `eventBus.publishAll` after `create` is
non-atomic.

## Decision

Use a **transactional Outbox**. Every event is written to the `Outbox` table
(`prisma/multi/schema.prisma`) before being handed to the transport, and a relay
worker (`OutboxEventBus` + `setInterval`) re-publishes any row with
`published=false`. The public `DomainEventBus` token is bound to
`OutboxEventBus`, wrapping the concrete transport (`KafkaEventBus` /
`InMemoryEventBus`, selected by `MESSAGING_DRIVER`).

For callers that manage their own transaction, `OutboxWriter.appendWithin(tx, …)`
persists the event in the same Prisma tx as the mutation (true exactly-once
write, at-least-once delivery).

## Consequences

- At-least-once delivery; a Kafka outage never loses an event (relay retries).
- Outbox table needs `prisma generate` + `migrate` and periodic cleanup of
  published rows (add a retention job in C.4/B).
- Consumers must be idempotent (C.2) because redelivery is possible.

## Consumer dedup contract (F.0.3)

Every `DomainEvent` now carries a stable `eventId` (UUID, defaulted in the
base class and persisted inside the Outbox `payload`). `KafkaConsumerService`
subscribes to `domain.<EventName>` topics (lazy `kafkajs` consumer, only when
`MESSAGING_DRIVER=kafka`) and guarantees each `eventId` is handled **at most
once** per process via an in-memory `Set`. Because the Outbox already gives
at-least-once delivery, this makes end-to-end delivery effectively exactly-once
for a single instance.

- Multi-instance / cross-replica dedup is deferred to F.1: when `REDIS_HOST` is
  set the consumer also records the `eventId` in Redis (`SET <id> NX PX`), so
  two replicas cannot both run the handler for the same event.
- Even without Redis, consumer handlers must stay idempotent (ADR contract):
  `eventId` dedup is a best-effort crash-safety layer, not the only guarantee.
- The immediate `publish()` path also marks the Outbox row `published` once the
  in-process dispatch succeeds, so the relay never re-delivers an event that was
  already delivered — eliminating the legacy double-dispatch between `publish`
  and the relay.

## See also

- Idempotency: `IdempotencyModule` + `IdempotencyInterceptor` (`@Idempotent()`), store overrideable by Redis.
- Chaos test proving at-least-once + idempotency: `libs/base/backend/src/lib/resilience.chaos.spec.ts`.
- Background jobs (BullMQ): `JobsModule` (`rotate-pii`, `audit-export`, `generic`).
- Back to the [docs hub](../README.md).
