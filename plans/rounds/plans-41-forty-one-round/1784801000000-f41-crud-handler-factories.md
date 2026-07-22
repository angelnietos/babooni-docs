<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F41-B1 � F�bricas de handlers CRUD CQRS</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Prioridad:** Alta  
**Estado:** Completado � f�brica creada y los 5 dominios thin (billing, inventory, projects, roles, tenants) migrados
**Dependencias:** F41-B4 (alinear tenantless elimina el fallback de paginaci�n de ra�z)

## Contexto

Los dominios thin (`billing`, `inventory`, `projects`, `tenants`) repiten los 5
handlers CRUD **byte-a-byte**, cambiando solo el token/tipo. Ejemplo real
(`billing/application/handlers/index.ts`):

```typescript
@Injectable() @CommandHandler(CreateBillingCommand)
export class CreateBillingHandler {
  constructor(@Inject(REPOSITORY_TOKENS.Billing) private readonly repo: BillingRepository) {}
  execute(c: CreateBillingCommand) { return this.repo.create(c.payload); }
}
// Update/Delete/Get/List id�nticos salvo nombre
```

El `ListXHandler` adem�s **duplica** el fallback de paginaci�n de
`CrudService.findPage` (ver F41-B4).

Archivos afectados:
- `domains/billing/application/handlers/index.ts`
- `domains/inventory/application/handlers/index.ts`
- `domains/projects/application/handlers/index.ts`
- `domains/tenants/application/handlers/index.ts` (variante tenantless, sin `tenantId`)

## Objetivo

Crear f�bricas gen�ricas en `shared/cqrs` que generen las clases de handler con
`@CommandHandler`/`@QueryHandler` aplicados din�micamente, parametrizadas por el
token de repositorio y las clases de command/query. Los dominios sin l�gica solo
declaran los tipos.

## Tareas

- [x] Crear `shared/cqrs/crud-handlers.factory.ts` con
      `makeCrudCommandHandlers({ token, create, update, remove })` y
      `makeCrudQueryHandlers({ token, get, list })` que devuelvan clases
      decoradas (usar `applyDecorators`/`Reflect` o `@CommandHandler(cmd)` sobre
      clases generadas). Contemplar variante tenantless.
- [x] Migrar `billing`, `inventory`, `projects`, `roles`, `tenants` a las f�bricas;
      eliminar el `index.ts` de handlers manual.
- [x] Verificar que el `ListXHandler` ya no reimplementa el fallback (usa la base
      de F41-B4 o `findPage` obligatorio).
- [x] Registrar los handlers generados en cada `<domain>.module.ts` sin cambiar
      su comportamiento observable.
- [x] Actualizar/portar los specs de handlers afectados.
- [x] `pnpm nx test base-backend` verde.

## Criterio de aceptaci�n

- Ning�n dominio thin mantiene handlers CRUD escritos a mano; se generan desde la
  f�brica.
- El tenant scoping (F37-H1) se conserva id�ntico (verificado por specs).
- Typecheck + lint + tests verdes en `base-backend`.

## Notas / riesgos

- NestJS `@CommandHandler` usa metadatos por clase; validar que las clases
  generadas din�micamente se registran correctamente en el `CqrsModule` (puede
  requerir nombres de clase �nicos o `Object.defineProperty(cls, 'name', ...)`).
- Los dominios con l�gica real (`clients`, `users`, `roles`, `audit`) mantienen
  sus handlers expl�citos; la f�brica es solo para los triviales.
