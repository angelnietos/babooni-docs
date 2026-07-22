<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Unit of Work / ALS in jobs and messaging (F38-H4)</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Problem

`TransactionContext` uses Node `AsyncLocalStorage`. HTTP request middleware and
command handlers that call `UnitOfWork.run()` / `runWithTransaction()` establish
that store. **BullMQ workers and Kafka consumers do not inherit request ALS** —
without an explicit wrapper, `TransactionContext.get()` is `undefined` and
repositories that rely on `getClient(prisma)` fall back to the root client
(or miss a transactional client entirely).

## Rule

| Context | Call |
|---------|------|
| Command handlers (atomic writes) | `uow.run(...)` or `uow.runWithTransaction(work)` (atomic default) |
| BullMQ `JobsProcessor`, Kafka / outbox local handlers | `uow.runWithTransaction(work, { atomic: false })` |
| Never | Loose `prisma.*` writes in processors without going through UoW/ports |

`{ atomic: false }` binds the root Prisma client into ALS **without** opening a
long-lived `$transaction` (bad for rotate-pii / multi-second jobs).

## Already wired

- `JobsProcessor.process` → `runWithTransaction(..., { atomic: false })`
- `KafkaConsumerService` handler dispatch → same
- `OutboxEventBus` local handlers → same

## Microservices TCP/gRPC (checklist / TODO)

Nest microservice transports also drop request ALS. Until a shared interceptor
exists:

1. Prefer **HTTP composition roots** (`api` / `api-single`) for transactional
   writes.
2. If a gRPC/TCP handler must touch repos that use `TransactionContext`, wrap
   the handler body in `UnitOfWork.runWithTransaction` (same as jobs).
3. **TODO (acotado):** add a Nest interceptor on microservice apps that calls
   `runWithTransaction(..., { atomic: false })` for every inbound message —
   track under a future harden lote; do not invent a second ALS store.

## Specs

```bash
pnpm exec jest -c libs/base/backend/jest.config.ts \
  src/lib/shared/cqrs/unit-of-work.spec.ts \
  src/lib/platform/ops/jobs/jobs.processor.spec.ts \
  src/lib/crosscutting/messaging/kafka.consumer.service.spec.ts
```

Assert: ALS visible **inside** the wrapper; `get()` is `undefined` **outside**.
