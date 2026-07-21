# F43-F1 — Limpiar `as any` y casts laxos en specs backend

**Prioridad:** Baja  
**Estado:** Pendiente  
**Dependencias:** F43-B1 (migración controller specs a harness elimina muchos `as any`)

## Contexto

Quedan casts `as any` y `as unknown as` en specs de `base-backend` que F41-F3
y F42-F1 no abordaron completamente porque dependen de la migración a harness
de F43-B1 / F43-B2.

Tras F43-A1 (facade genérica) también se redujeron `@ts-expect-error TS2416`
pero pueden quedar `as any` residuales en:
- `clients.service.spec.ts` — mocks de `TenantContext`
- `users.service.spec.ts` — mocks de `TenantContext`
- `settings.service.spec.ts` — mocks de `TenantContext`
- Cualquier spec que mockee `CommandBus`/`QueryBus` con `as any`

## Objetivo

Sustituir todos los `as any` en specs de `base-backend` por tipos
explícitos o DTOs de `@base/shared`.

## Tareas

- [ ] Inventariar todos los `as any` en `libs/base/backend/src/**/*.spec.ts`.
- [ ] Sustituir `as any` en controller specs por tipos de DTO
      (`CreateClientDto`, `UpdateClientDto`, etc.) o por un
      `AuthedRequest` tipado (F43-B1 elimina la mayoría).
- [ ] Sustituir `as any` en repository specs por tipos de dominio (F43-B2
      elimina la mayoría).
- [ ] Sustituir `as any` en service specs por mocks tipados de
      `CommandBus`/`QueryBus`/`TenantContext`.
- [ ] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cero `as any` en specs de `base-backend`.
- Typecheck + tests verdes.

## Notas / riesgos

- Algunos `as any` pueden ser necesarios temporalmente para mocks de
  `TenantContext`; priorizar los eliminables sin introducir tipos
  innecesarios.
