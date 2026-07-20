# Backend use-case / application tests (F36-P1-B / F38-H5)

## Policy

| Layer | Required tests | Notes |
|-------|----------------|-------|
| **Use-case / CQRS handlers** (`**/application/handlers/**`) | Unit spec with **mocked ports** (repos, event bus, policies) | Every **new** handler or application service must ship a `*.spec.ts` |
| **Domain services / mappers** | Keep existing mapper gate + domain specs | Pure logic, no Nest DI |
| **Controllers** | Smoke or contract test optional for complex DTOs | Prefer thin controllers |
| **Integration** (Prisma / Kafka / Redis) | Nightly / optional CI — **not** a daily local blocker | |

## Thin handlers (F38-H5) — no Prisma in `application/handlers`

Handlers are **orchestration only**: load ports, call domain services, emit
events, wrap writes in `UnitOfWork` when needed. They must **not** import
Prisma (`PrismaService`, `PrismaClient`, `@prisma/*`, `@base/prisma-*`,
`PRISMA_SERVICE`).

| Put logic here | Not here |
|----------------|----------|
| `domain/` entities, domain services, policies | Handler bodies with business rules |
| `ports/` repository interfaces | Direct `prisma.*.findMany` in handlers |
| `adapters/persistence/` Prisma repos | Copying CQRS folders into plantillas |

**Gate:** `pnpm check:domain-conventions` fails if any
`libs/base/backend/src/lib/domains/*/application/handlers/**` imports Prisma.

**Plantillas:** extend a facade or override a repository token — do **not** copy
the full CQRS tree. See
[`libs/arquetipos/backend/README.md`](../../libs/arquetipos/backend/README.md)
(“extend / decorate”, “What not to do”).

## Commands (no Nx daemon)

```bash
# Full @base/backend suite
pnpm exec jest -c libs/base/backend/jest.config.ts

# Coverage (mappers + application handlers)
pnpm exec jest -c libs/base/backend/jest.config.ts --coverage
```

If `nx test base-backend` hangs, use the jest command above (see [nx-daemon.md](../runbooks/nx-daemon.md)).

## Coverage gate

`libs/base/backend/jest.config.ts` collects coverage from:

- Domain mappers (`**/domain/*mappers.ts`) — stay high via existing specs
- Wave-1 application handlers (clients create/list/get, users create/get) — expand the list as more specs land

Global threshold starts at **70%** statements/lines (realistic combined gate). Raise toward 85% over time; do not drop mapper quality to chase the number.

## App / e2e testing (F38-C22 / C23 / C24)

| Layer | Required tests | Notes |
|-------|----------------|-------|
| **Backend apps** (`apps/*/backend`) | 1 AppModule test per product | Verifies the NestJS composition root compiles with mocked external deps |
| **Frontend apps** (`apps/*/frontend/*`) | 1 e2e smoke per product | Playwright: login + 1-2 core routes |
| **MF host** | 1 e2e smoke | Verifies React remotes mount inside Angular host |
| **Jest configs** | `collectCoverageFrom` + `coverageThreshold` | Realistic thresholds: 60% statements/lines, 50% branches |

### Coverage thresholds by project type

| Project type | Statements | Branches | Functions | Lines |
|-------------|-----------|---------|----------|-------|
| Base backend (libs) | 70 | 55 | 70 | 70 |
| Product apps / libs | 60 | 50 | 60 | 60 |

### Commands

```bash
# Unit tests (Nx or direct jest)
pnpm nx test verifactu-crm-api
pnpm nx test verifactu-worker

# E2E (Playwright)
pnpm exec playwright test -c apps/arquetipos/frontend/angular/microfrontend/mf-host-e2e/playwright.config.mts --project=chromium
pnpm exec playwright test -c apps/clientes/josanz/frontend/josanz-e2e/playwright.config.mts --project=chromium

# Coverage baseline (re-run after each wave)
node tools/scripts/report-test-gaps.mjs
```
