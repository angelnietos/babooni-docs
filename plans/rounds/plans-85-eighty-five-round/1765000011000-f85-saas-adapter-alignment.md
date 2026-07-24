# F85-B — SaaS / worker adapter alignment

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Eliminar imports directos de `PrismaPg` fuera del kernel
(`libs/base/backend/.../prisma/adapters.ts`).

## Acciones

- [ ] Auditar `libs/productos-saas/**` y `apps/productos-saas/**` por `PrismaPg`
- [ ] Sustituir por `resolveAndCreateAdapter` / `createAdapterForProvider`
- [ ] Ajustar seeds (`verifactu-crm-api/prisma/seed.ts`, workers)

## Criterios

- [ ] `rg PrismaPg` solo en `adapters.ts` (+ mocks de test)
- [ ] typecheck proyectos SaaS afectados verde
