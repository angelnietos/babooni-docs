# F43-B3 — Migrar imports profundos a barrel exports

**Prioridad:** Media  
**Estado:** Pendiente  
**Dependencias:** Ninguna

## Contexto

El commit de lint de F41 añadió una regla que prohíbe imports con
`../../../../` o más niveles en `libs/base/backend/src/lib/domains/**/*.ts`.

Actualmente hay **decenas** de archivos que violan esta regla:

- `domains/audit/adapters/http/*.ts` — `../../../../platform/kernel/...`
- `domains/audit/adapters/persistence/*.spec.ts` — `../../../../platform/...`
- `domains/clients/adapters/http/*.spec.ts` — `../../../../platform/...`
- Y muchos más en `billing`, `inventory`, `projects`, `roles`, `settings`,
  `tenants`, `users`

Los imports apuntan a:
- `../../../../platform/kernel/common/auth/jwt.strategy`
- `../../../../platform/kernel/common/multi-tenant/tenant.guard`
- `../../../../platform/kernel/common/roles/roles.guard`
- `../../../../platform/kernel/common/decorators`
- `../../../../platform/kernel/prisma/prisma.tokens`
- `../../../../platform/kernel/security/pii-config`
- `../../../../platform/kernel/security/key-provider`
- `../../../../crosscutting/http/crud.controller`
- `../../../../crosscutting/http/api-controller`
- `../../../../shared/adapters/prisma.repository`
- `../../../../shared/di/tokens`

## Objetivo

Sustituir los imports profundos por imports vía barrel/shared exports,
eliminando la deuda de mantenibilidad y cumpliendo la regla de lint.

## Estrategia

1. **Barrel existente:** `libs/base/backend/src/lib/shared/index.ts` ya exporta
   varios símbolos. Ampliarlo para cubrir los imports cruzados que hoy usan
   rutas profundas.
2. **Barrel de dominio:** cada dominio puede tener un `index.ts` que re-exporte
   sus puertos, DTOs y mappers.
3. **Imports de platform/kernel:** si el símbolo es compartido por múltiples
   dominios, añadirlo al barrel de `shared/`. Si es específico de un dominio,
   re-exportarlo desde el barrel del dominio.

## Tareas

- [ ] Inventariar todos los imports `../../../../` en `domains/**/*.ts`.
- [ ] Decidir barrel de destino para cada import (shared vs dominio).
- [ ] Ampliar `libs/base/backend/src/lib/shared/index.ts` con los exports
      necesarios (`JwtAuthGuard`, `TenantGuard`, `RolesGuard`, etc.).
- [ ] Migrar los imports en batches (por dominio) y verificar lint.
- [ ] `pnpm nx lint base-backend` pasa sin errores de deep-import.
- [ ] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cero imports con `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`.
- Lint de `base-backend` verde.
- Tests verdes.

## Notas / riesgos

- Algunos imports pueden ser de tipos privados de módulos; en ese caso,
  mover el tipo a un barrel público o crear un re-export explícito.
- No romper el límite de profundidad del barrel (no crear ciclos).
