<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Runbook — Kafka / Redis outage recovery</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Both Kafka and Redis are **optional** dependencies (resilience-by-default).
The app stays `Ready` (HTTP 200) through their outage; only the database
downgrades readiness to `503`.

## Redis down

**Impact**
- Idempotency keys become per-instance (no cross-replica sharing).
- BullMQ jobs queue is unavailable (`NoopJobsService`); background jobs pause.
- `dependency_up{dependency="redis"}` = 0 → `RedisDown` alert.

**Recovery**
1. Restore Redis (`kubectl -n josanz rollout restart sts/redis` or operator).
2. Pods auto-reconnect on next request (lazy client + `enableOfflineQueue`).
3. Verify `dependency_up{dependency="redis"}` returns to 1 and
   `jobs_queue_size` drains.
4. No data loss: jobs resume from the queue once Redis is back.

## Kafka down

**Impact**
- Domain events buffer in the **Outbox** (`published=false`); the relay keeps
  retrying. No event is lost.
- `kafka_publish_errors_total` increments; `KafkaPublishFailing` alert fires.
- Consumers (`KafkaConsumerService`) idle; in-memory dedup `Set` empties on
  restart but Redis-backed dedup resumes.

**Recovery**
1. Restore the broker / fix `KAFKA_BROKERS`.
2. The outbox relay re-publishes buffered rows within `OUTBOX_INTERVAL_MS`
   (default 5s). `outbox_backlog` drains to 0.
3. Consumers reconnect on `onApplicationBootstrap`/rebalance and replay
   `domain.<EventName>` topics; dedup by `eventId` prevents double handling.
4. Verify `outbox_backlog` ≈ 0 and `kafka_published_total` resumes climbing.

## Both down simultaneously

- DB remains the only hard dependency. App serves reads/writes (local handlers
  run); only Kafka fan-out and background jobs pause. Once both recover, the
  outbox drains and jobs resume — fully self-healing.
- If a **forced** recovery is needed, restart the Deployment to clear in-memory
  buffers (outbox state lives in the DB, so no events are lost):
  `kubectl -n josanz rollout restart deploy/josanz`.
