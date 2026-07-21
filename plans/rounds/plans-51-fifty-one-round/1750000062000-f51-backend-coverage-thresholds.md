# F51-A3 — Cobertura backend audit/settings/tenants + gate

## Estado

listo para ejecutar

## Objetivo

F50 añadió ≥ 10 tests pero no midió cobertura formal. Instrumentar y fijar
umbral ≥ 80% en el scope audit / settings / tenants (o justificar gaps).

## Tareas

1. Configurar coverage en Jest de `base-backend` (collectCoverageFrom domains).
2. Script o target Nx: `pnpm nx test base-backend --coverage` (o script root).
3. Umbrales por path o global documentados en [testing-pyramid.md](../../../guides/testing-pyramid.md).
4. Opcional: gate CI soft (warn) → strict en F52 si es estable.
5. Rellenar gaps si coverage < 80% (handlers / mappers / interceptor).

## Criterios de aceptación

- [ ] Reporte coverage reproducible localmente.
- [ ] audit / settings / tenants ≥ 80% statements (o tabla de exclusiones).
- [ ] Doc de cómo leer el reporte en biblia/guides.
