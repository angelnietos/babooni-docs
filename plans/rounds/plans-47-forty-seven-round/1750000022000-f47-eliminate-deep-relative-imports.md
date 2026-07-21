# F47-A3 — Eliminar imports profundos en base-backend

## Estado

listo para ejecutar

## Objetivo

Eliminar los imports con `../../../../` o más niveles en
`libs/base/backend/src/lib/domains/**/*.ts` que violan la regla de lint
añadida en F41, sustituyéndolos por barrel exports.

## Tareas

1. Ejecutar `rg "\.\./\.\./\.\./\.\./" libs/base/backend/src/lib/domains -l` para
   listar archivos afectados.
2. Ampliar `libs/base/backend/src/lib/shared/index.ts` con los exports compartidos
   que hoy se importan por ruta profunda:
   - `JwtAuthGuard`
   - `TenantGuard`
   - `RolesGuard`
   - decoradores de auth/roles
   - `prisma.tokens`
   - `PiiConfig`
   - `KeyProvider`
3. Ampliar barrels de dominio específico cuando el símbolo no sea compartido.
4. Migrar imports en batches por dominio y ejecutar `pnpm nx lint base-backend`
   tras cada batch.
5. Ejecutar `pnpm nx test base-backend` al final.

## Criterios de aceptación

- [ ] Cero `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`.
- [ ] `pnpm nx lint base-backend` pasa sin errores de deep-import.
- [ ] `pnpm nx test base-backend` verde.
