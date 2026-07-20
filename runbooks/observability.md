# Observability runbook (F8-OBS1)

## Current stack

| Signal | Implementation |
|--------|----------------|
| Logs | `LoggingInterceptor` — JSON `{ requestId, traceparent, tenantId, userId, … }` |
| Metrics | Prometheus `/api/metrics` |
| Errors | Sentry filter (when `SENTRY_DSN` set) |
| Health | `/api/health`, `/api/health/ready` |
| Tracing seam | `TracingService` — real spans when `OTEL_ENABLED=true` + `@opentelemetry/api` |

## W3C trace context (baseline)

Every HTTP response includes:

- `x-request-id` — correlation id (reused from request or minted)
- `traceparent` — W3C trace context (propagated from inbound header or minted)

Downstream calls should forward both headers. `TracingService.injectContext()` adds
OTel propagation when enabled.

## Enable OpenTelemetry (optional)

```bash
OTEL_ENABLED=true
# install @opentelemetry/api + SDK in the deployment image
```

Jaeger/Tempo are **not** bundled in `docker-compose.dev.yml` yet — add when you need
visual traces in local dev.

## Smoke

```bash
curl -i http://localhost:3000/api/health
# expect x-request-id + traceparent response headers
```
