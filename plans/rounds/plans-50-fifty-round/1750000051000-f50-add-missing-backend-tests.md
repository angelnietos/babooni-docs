# F50-A2 — Añadir tests faltantes en dominios backend

## Estado

listo para ejecutar

## Origen

Carry de [F49-A4](../plans-49-forty-nine-round/1750000043000-f49-add-missing-backend-tests.md).

## Objetivo

Subir cobertura y casos edge en audit, settings, tenants (+ edges CRUD
billing/inventory/projects). Seguir [testing-pyramid.md](../../../guides/testing-pyramid.md)
y harnesses `@base/backend`.

## Dominios

| Dominio | Gaps |
|---------|------|
| Audit | `AuditActorInterceptor`, `log()` tenant null, seed vacío |
| Settings | key duplicada, paginación `ListSettings` |
| Tenants | slug duplicado, status suspended/archived |
| Billing / Inventory / Projects | edges `findPage` / DTO validation |

## Tareas

1. Audit: specs interceptor + service edge cases.
2. Settings: service/handler specs.
3. Tenants: extender handlers specs.
4. Opcional: 1–2 edges por dominio CRUD.
5. `pnpm nx test base-backend` tras cada dominio; coverage al final.

## Criterios de aceptación

- [ ] ≥ 10 tests nuevos.
- [ ] audit / settings / tenants ≥ 80% en el scope medido (o justificación en Resultado).
- [ ] `pnpm nx test base-backend` verde.
