# F42-F1 — Limpiar `as any` y casts laxos en specs backend

**Prioridad:** Baja  
**Estado:** Pendiente  
**Dependencias:** Ninguna

## Contexto

Quedan casts `as any` y `as unknown as` en specs de backend que F41-F3
no abordó porque estaban fuera de `libs/base/frontend`:

- `clients.controller.spec.ts:41,65,67` — `{ tenantId: 't1' } as any`,
  `{ name: 'Acme' } as any`, `{ name: 'New' } as any`
- `settings.controller.spec.ts` — similar
- `billing.controller.spec.ts`, `inventory.controller.spec.ts`,
  `projects.controller.spec.ts` — posiblemente similar

Además, el `req` en controller specs es `as any` en 9 archivos.

## Objetivo

Sustituir todos los `as any` en specs de `base-backend` por tipos
explícitos o DTOs de `@base/shared`.

## Tareas

- [ ] Inventariar todos los `as any` en `libs/base/backend/src/**/*.spec.ts`.
- [ ] Sustituir `as any` en controller specs por tipos de DTO
      (`CreateClientDto`, `UpdateClientDto`, etc.) o por un
      `AuthedRequest` tipado.
- [ ] Sustituir `as any` en repository specs por tipos de dominio.
- [ ] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cero `as any` en specs de `base-backend`.
- Typecheck + tests verdes.

## Notas / riesgos

- Algunos `as any` pueden ser necesarios temporalmente para mocks de
  `TenantContext`; priorizar los eliminables sin introducir tipos
  innecesarios.
