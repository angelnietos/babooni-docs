<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F50-A3 — Tipar int-specs / residual `as any` en tests backend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Origen

Residual de [F49-A2](../plans-49-forty-nine-round/1750000041000-f49-cleanup-any-casts-in-implementations.md).

## Resultado

Cero `as any` en `libs/base/backend/src/lib/domains/**/*.int-spec.ts`.

Patrón aplicado:

- mocks de tenant → `as unknown as TenantContext`
- Prisma client → `as unknown as InjectedPrismaClient`
- payloads create → `Omit<XDto, 'id'>` (users: `UserCreatePayload` + cast a port)

Dominios: audit, billing, clients, inventory, projects, settings, users.

## Criterios de aceptación

- [x] Cero `as any` en domain int-specs.
- [x] Suite unit verde (`base-backend`).
