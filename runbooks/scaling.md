# Runbook — Scaling

## HorizontalPodAutoscaler

The chart ships an HPA (`deploy/helm/josanz/templates/hpa.yaml`):

- `minReplicas` / `maxReplicas` per env (prod: 3–10).
- Primary signal: CPU `70%` average utilization.
- Optional custom metric: `outbox_backlog` average value `50` (prod) once the
  Prometheus Adapter is installed.

```bash
kubectl -n josanz get hpa
kubectl -n josanz describe hpa josanz
```

## Manual scale

```bash
kubectl -n josanz scale deploy/josanz --replicas=5
# or via Helm
helm upgrade josanz deploy/helm/josanz --set replicaCount=5 -n josanz
```

## Capacity signals

Watch the Grafana "Arquetipos API" dashboard:

- Sustained p99 latency > SLO → scale out or investigate downstream (DB/Redis).
- `outbox_backlog` climbing → relay can't keep up; scale out and check Kafka.
- `jobs_queue_size` growing → scale the BullMQ worker (separate Deployment) or
  Redis capacity.

## Notes

- Replicas are stateless (DB/Redis-backed). Idempotency + event dedup are
  shared via Redis (`IDEMPOTENCY_STORE`), so scaling horizontally is safe.
- Liveness/readiness probes keep unhealthy pods out of rotation during scale.
