# F51-B1 — Typecheck libs SaaS CRM (más allá de la app)

## Estado

listo para ejecutar

## Objetivo

F50 dejó verde `verifactu-platform` (app). Ampliar a libs
`libs/productos-saas/crm/frontend/angular/**` y backends CRM usados por la app
para que el canario no oculte deuda en packages.

## Proyectos objetivo (inicial)

- `@saas/clients-*`, `@saas/identity-*`, `@saas/invoices-*`, `@saas/verifactu-*`
  (api / data-access / shell / features presentes)
- Shared CRM FE si existe

## Tareas

1. Inventario `pnpm nx show projects --withTarget typecheck` filtrado `saas` + crm.
2. Batch typecheck; arreglar `moduleResolution` / deps como en F50 verifactu.
3. `pnpm check:exports-paths` tras cambios de exports.
4. Documentar exclusiones preexistentes en Resultado (no `skip` silencioso).

## Criterios de aceptación

- [ ] Typecheck verde en lista objetivo (o exclusiones justificadas).
- [ ] Sin regresión en `verifactu-platform` / `document-generator`.
