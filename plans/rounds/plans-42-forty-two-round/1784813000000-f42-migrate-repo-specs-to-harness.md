# F42-B3 — Migrar prisma repository specs a `describeAntiFuga`

**Prioridad:** Media  
**Estado:** Pendiente  
**Dependencias:** Ninguna

## Contexto

F41-B5 creó `shared/testing/prisma-repo.harness.ts` con `makeCrudDelegate()`
y `describeAntiFuga()` pero **no se migraron** los specs de persistencia.
Los 6 specs de repositorio Prisma repiten:

- `makeCrudDelegate` inline (`findFirst`, `findMany`, `count` como `jest.fn()`)
- `loadCtx()` con `jest.isolateModules`
- `buildRepo()` con `as never`
- Tests anti-fuga F37-H1 copiados byte-a-byte

Archivos afectados:
- `domains/audit/adapters/persistence/audit.prisma.repository.spec.ts`
- `domains/billing/adapters/persistence/billing.prisma.repository.spec.ts`
- `domains/clients/adapters/persistence/clients.prisma.repository.spec.ts`
- `domains/inventory/adapters/persistence/inventory.prisma.repository.spec.ts`
- `domains/projects/adapters/persistence/projects.prisma.repository.spec.ts`
- `domains/settings/adapters/persistence/settings.prisma.repository.spec.ts`

## Objetivo

Cada spec de repositorio usa `makeCrudDelegate()` y `describeAntiFuga()`,
reduciendo el boilerplate a la configuración específica del dominio.

## Tareas

- [ ] Ajustar `describeAntiFuga` si es necesario para soportar la firma
      de `repoFactory` de cada dominio (algunos requieren `TenantContext`,
      otros no).
- [ ] Migrar los 6 specs de repositorio al harness.
- [ ] Eliminar `loadCtx` y `buildRepo` inline de los specs migrados.
- [ ] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cada repo spec usa `makeCrudDelegate()` y `describeAntiFuga()`.
- Cero `loadCtx`/`buildRepo` inline.
- Mismo número de asserts efectivos (no perder cobertura anti-fuga).

## Notas / riesgos

- Algunos dominios tenantless (`roles`, `tenants`) usan `TenantlessPrismaRepository`;
  el harness debe soportar esa variante sin cambios.
