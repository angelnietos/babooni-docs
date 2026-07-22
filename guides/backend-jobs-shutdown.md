<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">BullMQ / Kafka graceful shutdown (F38-H11)</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Apps

Canary Nest apps call `app.enableShutdownHooks()` so `SIGTERM` runs
`onModuleDestroy` (Prisma disconnect, outbox relay timer clear, Kafka consumer
disconnect, BullMQ workers close):

- `apps/arquetipos/backend/monolith/api`
- `apps/arquetipos/backend/monolith/api-single`

## BullMQ lock / stalled recovery

`JobsProcessor` registers worker options:

| Option | Env | Default | Role |
|--------|-----|---------|------|
| `lockDuration` | `JOBS_LOCK_DURATION_MS` | 30000 | Hold lock while processing |
| `stalledInterval` | `JOBS_STALLED_INTERVAL_MS` | 30000 | Detect abandoned locks after hard kill |
| `maxStalledCount` | `JOBS_MAX_STALLED_COUNT` | 1 | Retries after stall before `failed` |

Enqueue uses `attempts: 3` + exponential backoff (`jobs.service.ts`). A thrown
error mid-`process()` (including "job interrupted") leaves the job retryable.

## Outbox when Redis / broker is down

Aligned with F37-H2: outbox rows stay `published=false`; relay logs + metrics;
no infinite enqueue loop when the transport fails — see
`OutboxEventBus.relayUnpublished`.
