# F52-B2 — Gate CI cobertura backend (soft → strict)

## Estado

listo para ejecutar

## Objetivo

F51-A3 instrumenta cobertura local (≥ 80% audit/settings/tenants). Esta ronda
lleva el umbral a **CI**: primero **soft** (warn / artifact, no fail), luego
opcional **strict** si el reporte es estable.

No bajar la barra inventada en A3; si A3 aún no midió, completar el mínimo
aquí (collectCoverageFrom + script root) antes del gate.

## Tareas

1. Confirmar script local reproducible (`pnpm nx test base-backend --coverage`
   o script root documentado en F51-A3 / testing-pyramid).
2. Añadir step en workflow CI (job `quality` o `verify`):
   - Generar reporte (lcov/json).
   - Publicar artifact.
   - Soft: `continue-on-error: true` o script que imprime WARN si < umbral.
3. Documentar en [testing-pyramid.md](../../../guides/testing-pyramid.md) y
   [ci-gates.md](../../../frontend/ci-gates.md) (o runbook backend si encaja mejor).
4. Si 2 semanas / N PRs estables: PR follow-up o subtarea para `strict`
   (fail job). No forzar strict el mismo día del soft.

## Criterios de aceptación

- [ ] Cobertura reproducible en CI (artifact o log).
- [ ] Soft gate cableado y documentado.
- [ ] Umbral alineado con F51-A3 (o tabla de exclusiones en Resultado).
- [ ] Strict diferido con criterio explícito (no “TODO eterno” sin dueño).
