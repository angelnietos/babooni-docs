# F52-B2 — Gate CI cobertura backend (soft → strict)

## Estado

completado

## Resultado

Job `quality` en `.github/workflows/ci.yml`:

1. `pnpm test:cov:check` con `continue-on-error: true` (soft).
2. Artifact `base-backend-coverage` (`lcov` + `coverage-summary.json`).
3. `::warning::` si el step falla.

Umbral = F51-A3 (audit/settings/tenants ≥80% stmts / ≥65% branches).

**Strict diferido:** dueño maintainers backend; promover a fail cuando soft no
dispare en main durante ~2 semanas / N PRs. Documentado en
[testing-pyramid.md](../../../guides/testing-pyramid.md) y
[ci-gates.md](../../../frontend/ci-gates.md).

## Criterios de aceptación

- [x] Cobertura reproducible en CI (artifact + log).
- [x] Soft gate cableado y documentado.
- [x] Umbral alineado F51-A3.
- [x] Strict diferido con criterio explícito (dueño + ventana ~2 semanas).
