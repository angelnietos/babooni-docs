<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F44-B1 — Corregir specs remanentes (test harness)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Eliminar los errores preexistentes del test harness que `nx typecheck base-backend` reporta en `*.spec.ts`.

## Entradas

- Reporte de `pnpm nx typecheck base-backend` actual.
- Directorio de specs: `libs/base/backend/src/lib/**/*.spec.ts`.

## Errores conocidos

- `audit.controller.spec.ts`, `billing.controller.spec.ts`, `clients.controller.spec.ts`, `inventory.controller.spec.ts`, `projects.controller.spec.ts`, `roles.controller.spec.ts`, `settings.controller.spec.ts`, `tenants.controller.spec.ts`:
  - Tipo `AnyConstructor` no compatible con constructores tipados.
- `billing.prisma.repository.int-spec.ts`, `inventory.prisma.repository.int-spec.ts`, `projects.prisma.repository.int-spec.ts`:
  - `PrismaMultiService` no compatible con `InjectedPrismaClient`.

## Tareas

1. Ajustar `libs/base/backend/src/lib/shared/testing/controller.harness.ts` para aceptar constructores tipados o usar `any` explícito en tests.
2. Corregir int-specs para castear `PrismaMultiService` a `InjectedPrismaClient` o usar helper de test adecuado.
3. Verificar con `pnpm nx typecheck base-backend` que no queden errores en `lib/**/*.spec.ts`.

## Criterios de aceptación

- [ ] `tsc -p libs/base/backend/tsconfig.spec.json --noEmit` pasa sin errores.
- [ ] `pnpm nx typecheck base-backend` pasa en `lib/**/*.spec.ts`.
- [ ] No se introducen cambios de comportamiento en `lib/`.

## Dependencias

- Ninguna; puede ejecutarse en paralelo con F44-A1.
