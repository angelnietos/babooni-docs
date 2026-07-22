<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-B3 � Helpers de wiring de m�dulos (repos + contribuci�n AI)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Alta  
**Estado:** Completado
**Dependencias:** F41-B4 (alinear tenantless elimina el `as unknown as`)

## Contexto

Los 9 `*.module.ts` repiten wiring con **tres formas distintas** de registrar el
mismo repositorio Prisma:

1. `clients`/`users`/`settings`: `providePrismaRepository(TOKEN, Class)` (can�nico).
2. `billing`/`inventory`/`projects`: registran la clase **y** un
   `{ provide: TOKEN, useExisting: Class }` a mano.
3. `roles`/`tenants`/`audit`: `useFactory` inline con
   `new TenantlessPrismaRepository(...) as unknown as XRepository` � factories
   casi id�nticas entre roles y tenants.

Adem�s, los 9 repiten la contribuci�n AI:

```typescript
{ provide: AI_QUERY_CONTRIBUTION, multi: true,
  useValue: [['x.list', ListX], ['x.get', GetX]] } as Provider
```

## Objetivo

Un �nico helper de registro de repositorio (tenant y tenantless) y un helper de
contribuci�n AI, eliminando las factories inline, el doble registro y el
`as unknown as`.

## Tareas

- [x] Extender `shared/di/provide-prisma-repository.ts` con
      `provideTenantlessPrismaRepository(token, delegateKey, mappers)` que cree
      el `TenantlessPrismaRepository` sin `as unknown as` (depende de F41-B4 para
      que el tenantless satisfaga el port).
- [x] Forzar que `billing`/`inventory`/`projects` usen `providePrismaRepository`
      (eliminar el `useExisting` manual).
- [x] Crear `shared/cqrs/provide-ai-queries.ts` con
      `provideAiQueries(prefix, { list, get })` que devuelva el `Provider`
      `AI_QUERY_CONTRIBUTION` multi.
- [x] Migrar los 9 m�dulos a los helpers.
- [x] `pnpm nx test base-backend` verde (incluye tests de contribuci�n AI).

## Criterio de aceptaci�n

- Una sola forma de registrar repositorios (tenant y tenantless) en todos los
  dominios base.
- Cero `as unknown as XRepository` en m�dulos.
- La contribuci�n AI se declara con un helper, no copy-paste.
- Typecheck + lint + tests verdes en `base-backend`.

## Notas / riesgos

- El registro AI usa `satisfies AiQueryContribution`; conservar ese tipado en el
  helper.
