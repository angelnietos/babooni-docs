<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="64" alt="Arquetipos" />
</p>

<h1 align="center">Architecture Decision Records</h1>

<p align="center">
  Decisiones irreversibles · formato <a href="https://adr.github.io/madr/">MADR</a>
</p>

<p align="center">
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Decision logs for the `arquetipos` platform, in [MADR](https://adr.github.io/madr/)
format. Each record captures **context, decision, and consequences** so future
changes can be evaluated against the original rationale.

> Empieza en la [biblia del monorepo](../README.md) para orientación, la pirámide de
> tests, variables de entorno y la lista «cosas que sorprenden».

## Index

| # | Title | Decision in one line |
|---|-------|----------------------|
| [0001](adr-0001-hexagonal-architecture.md) | Hexagonal architecture | Per-domain ports & adapters, layered `@base` / `@arquetipos` / `@josanz` |
| [0002](adr-0002-prisma-multi-single-tenancy.md) | Prisma multi/single tenancy | Two schemas, one `PrismaRepository`, `tenantId` scoping via `TenantContext` |
| [0003](adr-0003-aes-256-gcm-vs-kms.md) | AES-256-GCM vs KMS | App-layer envelope encryption with `kid`; `KeyProvider` pluggable to KMS |
| [0004](adr-0004-kafka-outbox.md) | Kafka + transactional Outbox | Outbox table + relay → at-least-once delivery; consumers must be idempotent |
| [0005](adr-0005-jwt-vs-keycloak.md) | JWT (propio) vs Keycloak | **Keycloak** as OIDC IdP; backend is a JWKS resource server, issues no JWT |
| [0006](adr-0006-frontend-layering.md) | Frontend layering & paridad | `shared/` presentacional, forma `layout/pages/components`, paridad Angular↔React, gobernanza en CI |
| [0007](adr-0007-http-trace-context.md) | HTTP trace context | Propagate W3C traceparent across FE→API |
| [0008](adr-0008-platform-scope-vs-mvp-client.md) | Platform vs MVP client | Default client = web SPA + monolith; Next/RN/Ionic/MF/MS are opt-in |
| [0009](adr-0009-cqrs-nest.md) | Nest CQRS in hexagonal kernel | Commands/Queries only; facades dispatch; AI-gateable writes |
| [0010](adr-0010-native-ui-lit-sot.md) | Lit native-ui SoT | `@base/native-ui` = cross-framework SoT; freeze framework-only primitives in base; wrappers + RN tokens |
| [0011](adr-0011-storybook-native-ui-first.md) | Storybook native-first | SB serve+build on native-ui; serve on base Angular/React UI; ownership by package |
| [0012](adr-0012-isomorphic-validation.md) | Isomorphic validation FE↔BE | class-validator SoT + shared predicates; Zod kit deferred |
| [0013](adr-0013-recalls-strangler-migration.md) | Recalls_v2 → cliente Ideauto | **Strangler** into `clientes/ideauto/recalls` (`@ideauto/*`); **not** SaaS (proposed) |

## How to use these

- Before changing a cross-cutting system (auth, encryption, events, tenancy, **UI SoT**),
  read its ADR and respect the stated **consequences**.
- To propose a reversal or a new cross-cutting decision, add a new
  `adr-NNNN-*.md` (copy the accepted format) and link it here. Keep the table
  above in sync.
- ADRs are **accepted** unless their status says otherwise; an ADR can be
  superseded by a newer one (note the `revisited` date in 0005).

## Enlaces

- [architecture/overview.md](../architecture/overview.md) — mapa mental apps vs libs
- [frontend/ui-strategy.md](../frontend/ui-strategy.md) — Lit + wrappers (operativo)
- [docs/README.md](../README.md) — biblia del monorepo
