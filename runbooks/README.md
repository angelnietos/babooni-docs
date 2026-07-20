# Runbooks — operación en producción

Procedimientos operativos para Josanz, plantillas Arquetipos y SaaS Verifactu. Cada runbook es un checklist; actualízalo cuando cambie la plataforma.

> Desarrollo local: [guides/local-development.md](../guides/local-development.md)  
> Arquitectura BD por app: [backend/backend-domain-convention.md](../backend/backend-domain-convention.md)

---

## Índice

### Datos y dependencias

- [Database migrations](./database-migrations.md) — schemas Prisma single/multi/CRM, env por app, migrate
- [pnpm workspace layout](./pnpm-layout.md) — `workspace:*`, hoisting, symlinks
- [Nx daemon](./nx-daemon.md) — hang diagnosis, `NX_DAEMON=false`, plugin workers
- [Nx remote cache](./nx-remote-cache.md) — Nx Cloud / team + CI cache (`nx connect`, `NX_CLOUD_ACCESS_TOKEN`)
- [Dependency overrides](./dependency-overrides.md) — `pnpm.overrides` + upgrade policy
- [Kafka / Redis outage](./kafka-redis-outage.md) — modo degradado, outbox, reconexión
- [PII backup & restore](./backup-restore-pii.md) — dump cifrado, rotación de claves
- [PII key rotation](./pii-key-rotation.md) — rotación online, mixed kids, retención

### Despliegue y seguridad

- [Deploy & rollback](./deploy.md) — Helm, ArgoCD, HPA
- [Secrets management](./secrets.md) — KMS, SealedSecrets, sin plaintext en git
- [Scaling](./scaling.md) — réplicas, capacidad

### Observabilidad

- [Observability](./observability.md) — logs JSON, `/metrics`, OTel, alertas Grafana

---

## Modelo de health (resilience-by-default)

| Endpoint | Comportamiento |
|----------|----------------|
| `GET /api/health` | Liveness — siempre `200` si el proceso vive |
| `GET /api/health/ready` | Readiness — `503` solo si **Postgres** caído; `200` + `degraded` si Redis/Kafka/KMS caídos |
| `GET /metrics` | Prometheus — `dependency_up`, `outbox_backlog`, colas |

Kubernetes mantiene el pod `Ready` en outage de dependencias opcionales (sigue sirviendo tráfico).

Dashboards y alertas: `deploy/observability/`.

---

## Matriz rápida — qué runbook abrir

| Incidente / tarea | Runbook |
|-------------------|---------|
| Migrar BD Josanz o crear `josanz` DB | [database-migrations.md](./database-migrations.md) |
| Nuevo microservicio con BD propia | [database-migrations.md](./database-migrations.md) + [guides/add-microservice.md](../guides/add-microservice.md) |
| `pnpm install` / módulo no resuelto | [pnpm-layout.md](./pnpm-layout.md) |
| Kafka caído, eventos atascados | [kafka-redis-outage.md](./kafka-redis-outage.md) |
| Rotar clave de cifrado PII | [secrets.md](./secrets.md) + [backup-restore-pii.md](./backup-restore-pii.md) |
| Release fallido | [deploy.md](./deploy.md) |
| 5xx alto / latencia | [observability.md](./observability.md) |

---

## Enlaces

- [docs/README.md](../README.md) — biblia
- [SERVICES.md](../../SERVICES.md) — dominios y rutas
- [adr/README.md](../adr/README.md) — decisiones cross-cutting
