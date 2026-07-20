# BullMQ / Kafka graceful shutdown (F38-H11)

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
