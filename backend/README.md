<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Backend</h1>

<p align="center">
  NestJS · hexagonal · CQRS · apps vs libs
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="./why-nest.md"><img alt="Why Nest" src="https://img.shields.io/badge/why-Nest-E0234E?style=flat-square" /></a>
  <a href="./backend-domain-convention.md"><img alt="Convention" src="https://img.shields.io/badge/convention-domains-14b8a6?style=flat-square" /></a>
</p>

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
