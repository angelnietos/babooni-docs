# F50-A3 — Tipar int-specs / residual `as any` en tests backend

## Estado

listo para ejecutar

## Origen

Residual de [F49-A2](../plans-49-forty-nine-round/1750000041000-f49-cleanup-any-casts-in-implementations.md).

## Objetivo

Eliminar o tipar `as any` en `*.int-spec.ts` y mocks Prisma de domains
(y, si es barato, messaging specs), usando `makeCrudDelegate` /
interfaces de delegate en vez de `as any`.

## Archivos (inicial)

- `*/adapters/persistence/*.prisma.repository.int-spec.ts` (audit, billing, clients, inventory, projects, settings, users)
- Specs messaging / jobs con `as any` si se tocan en el mismo PR

## Tareas

1. Introducir o reutilizar tipos de mock (`PrismaDelegate` / harness).
2. Migrar int-specs domain por domain.
3. No empeorar cobertura ni flaky tests.
4. `pnpm nx test base-backend` (+ integration si se corre en local con Postgres).

## Criterios de aceptación

- [ ] Cero `as any` en `libs/base/backend/src/lib/domains/**/*.int-spec.ts` (o ≤ N documentados con justificación).
- [ ] Suite unit verde; int-specs no regresan.
