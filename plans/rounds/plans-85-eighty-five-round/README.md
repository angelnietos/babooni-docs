<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F85 — Monorepo engine & base hardening</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <img alt="previa" src="https://img.shields.io/badge/prev-F78+F83-14b8a6?style=flat-square" />
</p>

## Estado

**Activa** · apertura 2026-07-24

> Eje: **mejorar el motor y la base del monorepo** (kernel Prisma multi-provider,
> gates DB, alineación SaaS a adapters, harness/seeds).  
> **Fuera de scope:** ejecución producto Ideauto — vive en
> [`docs/ideauto/migration/`](../../../ideauto/migration/).

## Contexto

F78 (parity FE/mobile) y F83 (provider detection + adapters + shadow schemas +
ADR 0014) cerradas. Quedan endurecimientos de plataforma:

1. `migrate deploy` / seeds reales por motor (no solo `prisma validate`).
2. Quitar `PrismaPg` directo en SaaS/worker/seed → factory kernel.
3. Harness Jest consciente de `DATABASE_PROVIDER`.
4. Docs/gates alineados con el motor.

## Objetivos clave

1. Path migrate + smoke CI documentado para SqlServer (y MySQL opcional).
2. Entrypoints SaaS usan `createAdapterForProvider` / `resolveAndCreateAdapter`.
3. Harness + seeds portability.
4. Biblia/runbooks sin contradicciones; cierre F85.

## Planes detallados

| ID | Plan | Foco |
|----|------|------|
| F85-A | [MSSQL/MySQL migrate + CI](1765000010000-f85-db-migrate-ci-matrix.md) | Deploy real + matrix |
| F85-B | [SaaS adapter alignment](1765000011000-f85-saas-adapter-alignment.md) | Quitar PrismaPg sueltos |
| F85-C | [Portability hardening](1765000012000-f85-portability-hardening.md) | Harness + seeds |
| F85-D | [Docs, gates y cierre](1765000013000-f85-docs-gates-close.md) | Hub + biblia |

## Checklist de cierre F85

- [ ] F85-A migrate path MSSQL (y MySQL opcional) + CI documentado
- [ ] F85-B SaaS/worker usan `createAdapterForProvider`
- [ ] F85-C harness + seeds portability
- [ ] F85-D docs + hub; **F85 cerrada**

## Fuera de esta ronda

| Tema | Dónde |
|------|-------|
| Ideauto Recalls M0–M6 | [docs/ideauto/migration/](../../../ideauto/migration/) |
| Narrativa Recalls | [docs/ideauto/recalls/](../../../ideauto/recalls/) |

## Predecesoras

- F78 / F83 / F84 — cerradas (git only)
- ADR: [0014](../../../adr/adr-0014-database-provider-portability.md)
