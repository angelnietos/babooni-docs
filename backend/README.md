# Backend — índice

Cómo funciona el backend del motor: NestJS, hexagonal, CQRS, apps vs libs, y
**por qué Nest y no Express**.

## Lectura recomendada

| Orden | Doc | Contenido |
|-------|-----|-----------|
| 1 | [why-nest.md](./why-nest.md) | Nest vs Express — decisión de plataforma |
| 2 | [how-it-works.md](./how-it-works.md) | Request lifecycle, módulos, composition roots |
| 3 | [backend-domain-convention.md](./backend-domain-convention.md) | Slugs, empaquetados, matriz BD por app |
| 4 | [../architecture/backend-deep-dive.md](../architecture/backend-deep-dive.md) | Hex + CQRS al detalle |
| 5 | [../architecture/domain-lifecycle.md](../architecture/domain-lifecycle.md) | Dominio E2E con FE |

## Guías de acción

- [add-backend-domain.md](../guides/add-backend-domain.md)
- [add-microservice.md](../guides/add-microservice.md)
- [ai-cqrs-policy.md](../guides/ai-cqrs-policy.md)
- [testing-pyramid.md](../guides/testing-pyramid.md)
- [database-migrations.md](../runbooks/database-migrations.md)

## ADRs clave

| ADR | Tema |
|-----|------|
| [0001](../adr/adr-0001-hexagonal-architecture.md) | Hexagonal |
| [0002](../adr/adr-0002-prisma-multi-single-tenancy.md) | Tenancy Prisma |
| [0004](../adr/adr-0004-kafka-outbox.md) | Outbox / Kafka |
| [0005](../adr/adr-0005-jwt-vs-keycloak.md) | Keycloak |
| [0009](../adr/adr-0009-cqrs-nest.md) | CQRS Nest |
