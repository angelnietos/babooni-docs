<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-B5 � Helpers de test backend (dedup de specs)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Media  
**Estado:** Completado � `shared/testing` con `prisma-repo.harness.ts` y `controller.harness.ts` creados y usados
**Dependencias:** Ninguna

## Contexto

Los specs de persistencia y de controllers repiten los mismos helpers.

**Persistencia (`*.prisma.repository.spec.ts` en 7 dominios):**

```typescript
type Delegate = { findFirst: jest.Mock; findMany: jest.Mock; count: jest.Mock };
function loadCtx() { /* jest.isolateModules + require tenant.context, id�ntico */ }
function buildRepo(delegate, ctx) { return new XPrismaRepository({ x: delegate } as never, ctx); }
```

Y el cuerpo de los tests "anti-fuga F37-H1" (findById scopes where / no leak
tenant B) est� copiado en `billing`/`inventory`/`projects`/`settings`/`clients`/
`audit`, cambiando solo ids y campos.

**Controllers (`*.controller.spec.ts` en 9 dominios):**

```typescript
const req = { tenantId: 't1', user: { tenantId: 't1' } } as any;
.overrideGuard(JwtAuthGuard).useValue({ canActivate: () => true })
.overrideGuard(TenantGuard).useValue({ canActivate: () => true })
.overrideGuard(RolesGuard).useValue({ canActivate: () => true })
```

## Objetivo

Un m�dulo `shared/testing` (no incluido en el build de producci�n) con helpers
reutilizables para specs de repositorio y de controller.

## Tareas

- [x] Crear `shared/testing/prisma-repo.harness.ts` con `makeCrudDelegate()`,
      `makeTenantCtxLoader()` y un `describeAntiFuga(RepoClass, sampleRow)`
      parametrizable.
- [x] Crear `shared/testing/controller.harness.ts` con
      `buildControllerTestModule({ controller, providers })` que aplique los 3
      `overrideGuard` y devuelva `{ controller, req }` (punto �nico para futuros
      guards: permissions, rate-limit).
- [x] Migrar los 7 specs de persistencia y los 9 de controller a los helpers.
- [x] Asegurar que `shared/testing` queda excluido de `tsconfig.lib.json` (solo
      spec) para no filtrar utilidades de test al bundle.
- [x] `pnpm nx test base-backend` verde (mismo n� de asserts efectivos).

## Criterio de aceptaci�n

- `loadCtx`/`buildRepo`/`Delegate`/`overrideGuard`/`req` viven en un �nico lugar.
- Los specs de dominio se reducen a datos + expectativas espec�ficas.
- Typecheck (spec) + lint + tests verdes en `base-backend`.

## Notas / riesgos

- Sustituir el `req ... as any` por un tipo `AuthedRequest` de prueba tipado.
- No reducir cobertura: los tests anti-fuga deben seguir ejecut�ndose por dominio.
