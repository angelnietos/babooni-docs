# F41-B5 — Helpers de test backend (dedup de specs)

**Prioridad:** Media  
**Estado:** Completado — `shared/testing` con `prisma-repo.harness.ts` y `controller.harness.ts` creados y usados
**Dependencias:** Ninguna

## Contexto

Los specs de persistencia y de controllers repiten los mismos helpers.

**Persistencia (`*.prisma.repository.spec.ts` en 7 dominios):**

```typescript
type Delegate = { findFirst: jest.Mock; findMany: jest.Mock; count: jest.Mock };
function loadCtx() { /* jest.isolateModules + require tenant.context, idéntico */ }
function buildRepo(delegate, ctx) { return new XPrismaRepository({ x: delegate } as never, ctx); }
```

Y el cuerpo de los tests "anti-fuga F37-H1" (findById scopes where / no leak
tenant B) está copiado en `billing`/`inventory`/`projects`/`settings`/`clients`/
`audit`, cambiando solo ids y campos.

**Controllers (`*.controller.spec.ts` en 9 dominios):**

```typescript
const req = { tenantId: 't1', user: { tenantId: 't1' } } as any;
.overrideGuard(JwtAuthGuard).useValue({ canActivate: () => true })
.overrideGuard(TenantGuard).useValue({ canActivate: () => true })
.overrideGuard(RolesGuard).useValue({ canActivate: () => true })
```

## Objetivo

Un módulo `shared/testing` (no incluido en el build de producción) con helpers
reutilizables para specs de repositorio y de controller.

## Tareas

- [x] Crear `shared/testing/prisma-repo.harness.ts` con `makeCrudDelegate()`,
      `makeTenantCtxLoader()` y un `describeAntiFuga(RepoClass, sampleRow)`
      parametrizable.
- [x] Crear `shared/testing/controller.harness.ts` con
      `buildControllerTestModule({ controller, providers })` que aplique los 3
      `overrideGuard` y devuelva `{ controller, req }` (punto único para futuros
      guards: permissions, rate-limit).
- [x] Migrar los 7 specs de persistencia y los 9 de controller a los helpers.
- [x] Asegurar que `shared/testing` queda excluido de `tsconfig.lib.json` (solo
      spec) para no filtrar utilidades de test al bundle.
- [x] `pnpm nx test base-backend` verde (mismo nº de asserts efectivos).

## Criterio de aceptación

- `loadCtx`/`buildRepo`/`Delegate`/`overrideGuard`/`req` viven en un único lugar.
- Los specs de dominio se reducen a datos + expectativas específicas.
- Typecheck (spec) + lint + tests verdes en `base-backend`.

## Notas / riesgos

- Sustituir el `req ... as any` por un tipo `AuthedRequest` de prueba tipado.
- No reducir cobertura: los tests anti-fuga deben seguir ejecutándose por dominio.
