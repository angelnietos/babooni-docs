<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Observability runbook</h1>

<p align="center">
  <img alt="runbook" src="https://img.shields.io/badge/runbook-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Procedimiento operativo de señales (logs, métricas, trazas, alertas).

## Stack actual

| Señal | Implementación |
|-------|----------------|
| Logs | `LoggingInterceptor` — JSON `{ requestId, traceparent, tenantId, userId, … }` |
| Métricas | Prometheus `/api/metrics` (también documentado como `/metrics` según app) |
| Errores | Sentry filter si `SENTRY_DSN` |
| Health | `/api/health` (liveness), `/api/health/ready` (readiness) |
| Tracing | `TracingService` — spans si `OTEL_ENABLED=true` + `@opentelemetry/api` |

Dashboards / alertas: `deploy/observability/`.

## W3C trace context

Cada respuesta HTTP incluye:

- `x-request-id` — correlación (reusa inbound o mint)
- `traceparent` — W3C (propaga o mint)

Downstream debe reenviar ambos. `TracingService.injectContext()` añade
propagación OTel cuando está activo. ADR: [0007](../adr/adr-0007-http-trace-context.md).

## OpenTelemetry (opcional)

```bash
OTEL_ENABLED=true
# SDK OTel en la imagen de deploy
```

Jaeger/Tempo **no** van en `docker-compose.dev.yml` por defecto — añadirlos
cuando el equipo necesite trazas visuales en local.

## Onboarding SRE — checklist

1. Confirmar scrape de `/api/metrics` en el cluster.
2. Importar dashboards bajo `deploy/observability/`.
3. Alertas mínimas: `ApiHigh5xxRate`, `OutboxBacklogGrowing`, readiness Postgres.
4. Runbook de outage Kafka/Redis: [kafka-redis-outage.md](./kafka-redis-outage.md).
5. Tras deploy: 10 min de watch en 5xx + outbox ([deploy.md](./deploy.md)).

## Smoke local

```bash
curl -i http://localhost:3000/api/health
curl -i http://localhost:3000/api/health/ready
# expect x-request-id + traceparent
```

## Enlaces

- [docs/README.md](../README.md)
- [deploy.md](./deploy.md)
- [scaling.md](./scaling.md)
