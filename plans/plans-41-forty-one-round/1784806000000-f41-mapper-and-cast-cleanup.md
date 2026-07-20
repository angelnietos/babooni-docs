# F41-B6 — Limpieza de mappers, casts e imports

**Prioridad:** Media  
**Estado:** Completado
**Dependencias:** Ninguna

## Contexto

Tres focos de ruido de bajo riesgo:

1. **Mappers `toXxxUpdate` repetidos.** Todos son un spread condicional:
   ```typescript
   export const toXUpdate = (data) => ({
     ...(data.a !== undefined ? { a: data.a } : {}),
     ...(data.b !== undefined ? { b: data.b } : {}),
   });
   ```
   Y los `toXxxDto` de `billing`/`inventory`/`setting` son mapeos 1:1.

2. **Casts `as never` en producción** en repos thin:
   - `billing.prisma.repository.ts:19` ? `prisma.billing as never`
   - `inventory.prisma.repository.ts:20` ? `prisma.inventory as never`
   - `projects.prisma.repository.ts:19` ? `prisma.project as never`

   Conviven con el patrón correcto `prisma.setting as PrismaDelegate`.

3. **Import `PRISMA_SERVICE`/`InjectedPrismaClient` inconsistente:** unos
   dominios importan de `prisma.tokens`, otros de `prisma.module` (ambos
   funcionan porque el module re-exporta, pero es ruido).

## Objetivo

Normalizar mappers, casts e imports a una sola convención.

## Tareas

- [x] Crear util `shared/domain/pick-defined.ts` con
      `pickDefined(data, keys)` y reescribir todos los `toXxxUpdate` como una
      línea: `pickDefined(data, ['status', 'amount'])`.
- [x] Para los DTO 1:1 (`billing`, `inventory`, `setting`) valorar un
      `identityDto` cuando row y DTO coinciden en forma (documentar el criterio;
      no forzar donde haya normalización real como `users`/`clients`).
- [x] Reemplazar `prisma.x as never` por `prisma.x as PrismaDelegate` en los 3
      repos thin.
- [x] Fijar convención de imports: tokens siempre desde `prisma.tokens`;
      `PrismaModule` solo cuando se importa el módulo Nest. Migrar los ~10
      archivos divergentes.
- [x] `pnpm nx test base-backend` verde.

## Criterio de aceptación

- Cero `as never` en repositorios de producción.
- `toXxxUpdate` no repite el patrón de spread condicional a mano.
- Import de tokens Prisma consistente en todos los dominios base.
- Typecheck + lint + tests verdes en `base-backend`.

## Notas / riesgos

- `pickDefined` debe preservar el tipado (`Partial<TDto>` ? objeto con solo las
  claves definidas); usar genéricos con `keyof`.
- El `identityDto` solo aplica si NO hay transformación (fechas a ISO, defaults de
  tenant, etc.). En caso de duda, dejar el mapper explícito.
