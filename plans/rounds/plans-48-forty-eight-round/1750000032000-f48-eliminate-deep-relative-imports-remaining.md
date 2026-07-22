<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F48-A3 — Eliminar imports profundos restantes en base-backend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

Se sustituyeron todos los imports profundos (`../../../../`) en
`libs/base/backend/src/lib/domains` por barrel imports desde `@base/backend`.
Los símbolos migrados incluyen:
- Guards y harness de testing: `JwtAuthGuard`, `TenantGuard`, `RolesGuard`, `buildControllerTestModule`, `AuthedRequest`.
- Servicios: `PasswordResetService`, `DomainEventBus`, `TenantContext`.
- Filtros y DTOs: `DomainExceptionFilter`, `QueryOptions`, `KeyProvider`, `PiiConfig`, `UnitOfWork`.
- Repos y mocks: `PrismaMultiService`, `InjectedPrismaClient`, `PrismaRepository`, `makeCrudDelegate`, `describeAntiFuga`.
- Tokens y factories: `REPOSITORY_TOKENS`, `makeCrudCommandHandlers`, `makeCrudQueryHandlers`.

Validación:
- `pnpm nx lint base-backend`: pasa sin errores de deep-import.
- `pnpm nx test base-backend`: 109 suites, 306 tests passed.
- `grep '../../../..'` en `libs/base/backend/src/lib/domains/**/*.ts`: 0 coincidencias.

## Objetivo

Cerrar la deuda de imports profundos que quedó fuera de F47-A3. F47 migró
los handlers `index.ts` a barrel imports, pero quedan:
- Controller specs que importan guards/harness por ruta profunda.
- Repo specs que importan `TenantContext`/`makeCrudDelegate` por ruta profunda.
- Int-specs que importan `PrismaMultiService` por ruta profunda.
- Algunos dominios que importan `QueryOptions` por ruta profunda.

## Tareas

1. Ejecutar `grep -r "\.\./\.\./\.\./\.\./" libs/base/backend/src/lib/domains -l -g "*.ts"` para listar archivos restantes.
2. Ampliar barrels existentes en `@base/backend` con los símbolos que hoy se importan por ruta profunda:
   - `JwtAuthGuard`, `TenantGuard`, `RolesGuard` desde platform/kernel.
   - `PrismaMultiService`, `PRISMA_SERVICE` desde platform/kernel/prisma.
   - `DomainExceptionFilter` desde crosscutting/http.
   - `PasswordResetService` desde platform/kernel/common/auth.
3. Migrar imports en batches por dominio:
   - Batch 1: controller specs (clients, users, settings, roles, projects, inventory, billing, audit, tenants).
   - Batch 2: repo specs + int-specs.
   - Batch 3: handler/queries index.ts restantes.
4. Ejecutar `pnpm nx lint base-backend` tras cada batch.
5. Ejecutar `pnpm nx test base-backend` al final.

## Criterios de aceptación

- [ ] Cero `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`.
- [ ] `pnpm nx lint base-backend` pasa sin errores de deep-import.
- [ ] `pnpm nx test base-backend` verde.
