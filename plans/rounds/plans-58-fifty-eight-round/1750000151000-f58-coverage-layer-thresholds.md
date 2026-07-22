# F58-A2 — Umbrales / lectura de coverage por capa

## Estado

completado

## Objetivo

Definir **cómo leer** el informe (por proyecto vs global vs BE gate) y, si aporta,
umbrales **opt-in** por tag (`runtime:backend`, UI tokens) — no un % único monorepo.

## Entregables

1. Tabla en jest-coverage.md: qué gate aplica a qué capa.
2. Opcional: script `check-coverage-thresholds.mjs` leyendo `coverage/*/coverage-summary.json`
   para allowlist de proyectos (p. ej. `base-ui-tokens` lines ≥ X).
3. No tocar `test:cov:check` (audit/settings/tenants) salvo documentar relación.

## Criterios de aceptación

- [x] Docs claras “cómo leer el informe”.
- [x] Si hay script de umbrales: soft en CI o solo local documentado.
- [x] Gate BE dominio intacto.

## Depende de

F58-A1 (strict truth primero).
