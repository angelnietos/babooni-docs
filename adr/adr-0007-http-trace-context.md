# ADR 0007: HTTP trace context baseline

- Status: accepted
- Date: 2026-07-12

## Context

Services already emit structured logs with `requestId` and expose a `TracingService`
seam for OpenTelemetry. Cross-service debugging still lacked a standard propagation
header between gateway, monolith, and SaaS APIs.

## Decision

1. **`LoggingInterceptor`** propagates or mints W3C **`traceparent`** on every HTTP
   request and echoes it (with **`x-request-id`**) on the response.
2. Logs include both `requestId` and `traceparent`.
3. Full OTel spans remain opt-in via `OTEL_ENABLED=true` and `@opentelemetry/api`
   (see `TracingService`).

## Consequences

- No hard dependency on OTel SDK in the default build.
- Downstream HTTP clients should forward `traceparent` + `x-request-id`.
- Visual trace UI (Jaeger/Tempo) is optional follow-up in `docker-compose.dev.yml`.

## See also

- [runbooks/observability.md](../runbooks/observability.md)
- ADR 0002 (Prisma), ADR 0006 (frontend layering)
