# F47-A2 — Migrar repo specs a harness

## Estado

listo para ejecutar

## Objetivo

Completar la migración de los ~6 repo specs de `base-backend` al harness
`buildRepoTestModule` (o patrón equivalente), eliminando el boilerplate repetido
de `makeCrudDelegate`/`loadCtx`/`buildRepo`.

## Tareas

1. Abrir `libs/base/backend/src/lib/domains/clients/adapters/persistence/clients.prisma.repository.spec.ts`.
2. Reemplazar el bootstrap manual del módulo por el helper de harness.
3. Eliminar helpers locales `makeCrudDelegate`, `loadCtx`, `buildRepo`.
4. Repetir para los dominios restantes con repo specs pendientes:
   - `billing`
   - `inventory`
   - `projects`
   - `roles`
   - `settings`
   - `tenants`
5. Ejecutar `pnpm nx test base-backend` para confirmar que pasa.

## Criterios de aceptación

- [ ] Cada repo spec usa el helper de harness.
- [ ] Cero `makeCrudDelegate`/`loadCtx`/`buildRepo` inline en specs.
- [ ] `pnpm nx test base-backend` verde.
