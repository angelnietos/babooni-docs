# Ronda F55 — Carry F54: oleadas Lit, visual CI, coverage strict, Code Connect

## Estado

listo para ejecutar

> **Arranque:** tras F54 completada.
> F54 dejó native wrappers, tokens, SB native en CI, gate strict y paridad
> soft. F55 cierra el **carry explícito** y endurece calidad visual/cobertura.

## Dependencias externas

- F54 completada (migración P0, oleada 1 Lit, `@base/ui-tokens`, parity soft).
- Token Chromatic / Figma Code Connect opcionales — defer documentado si faltan.
- Ventana soft coverage BE (F52→F54) con métrica de PRs verdes.

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F55-A1 | [Oleada 2 Lit: select / icon / pagination](1750000120000-f55-lit-wave-select-icon-pagination.md) | Carry F54-A2 |
| F55-A2 | [Filter bar + residual inventario](1750000121000-f55-lit-wave-filter-bar-residual.md) | Carry inventario |
| F55-B1 | [Chromatic / visual regression (o Loki/PW)](1750000122000-f55-chromatic-visual-regression.md) | Carry F54-B1 |
| F55-B2 | [Parity visual smoke Playwright Angular↔React](1750000123000-f55-arquetipos-parity-visual-smoke.md) | Carry F54-B4 |
| F55-B3 | [A11y gaps: focus trap modal + tabs teclado](1750000124000-f55-native-ui-a11y-gaps.md) | Carry F54-B3 |
| F55-C1 | [Coverage BE soft → strict](1750000125000-f55-coverage-ci-strict.md) | Carry F54-D2 |
| F55-C2 | [`check:arquetipos-parity --strict` en CI](1750000126000-f55-arquetipos-parity-strict.md) | Carry F54-B4 |
| F55-C3 | [Jest workspace: preset + coverage libs/apps](1750000126500-f55-jest-workspace-coverage-rollout.md) | Infra Jest global |
| F55-D1 | [Figma Code Connect piloto button/input/alert](1750000127000-f55-figma-code-connect-pilot.md) | Carry F54-D1 |
| F55-D2 | [Storybook serve arquetipos UI](1750000128000-f55-arquetipos-storybook-targets.md) | **completado** |
| F55-E1 | [Documentación y cierre F55](1750000129000-f55-documentation.md) | Cierre |

## Objetivo

1. Completar oleada Lit residual (select, icon, pagination; filter bar si cabe).
2. Señal visual real (Chromatic o homólogo) sobre `base-native-ui`.
3. Smoke Playwright multi-stack + parity checker **strict**.
4. Cerrar gaps a11y críticos (modal trap, tabs arrow).
5. Promover coverage BE a fail en CI con evidencia.
6. Rollout Jest shared preset + coverage a libs/apps (merge `coverage/global`).
7. Piloto Code Connect o defer con token ausente.
8. Targets Storybook arquetipos UI huérfanos.

## Orden de ejecución

1. **A1** oleada select/icon/pagination.
2. **B3** a11y gaps (puede ir en paralelo a A*).
3. **C1** coverage BE strict cuando haya N PRs verdes post-F54.
4. **C3** Jest preset/coverage rollout (puede ir en paralelo a C1; no mezclar umbrales).
5. **B1 + B2** visual (Chromatic / Playwright) tras SB native estable.
6. **C2** parity strict tras B2 o con allowlist menguante.
7. **D1 / D2 / A2** según acceso Figma y capacidad.
8. **E1** cierre; remove deprecated → **F56** (no en esta ronda).

## Riesgos

- Select CE complejo (teclado, listbox) — no inventar HTML en features.
- Chromatic coste/token — soft/nightly primero.
- Coverage flaky → no promover sin métrica.
- Code Connect sin Figma access → matriz ya existe; no bloquear ronda.

## Criterios de aceptación (ronda)

- [ ] Oleada 2 con ≥2 CEs nuevos (select y/o icon y/o pagination) + wrappers.
- [ ] Estrategia visual elegida e integrada **o** defer con motivo.
- [ ] Coverage BE strict **o** defer F56 con conteo de PRs.
- [ ] Jest shared preset en mayoría de `test` targets **o** exclusiones en F55-C3.
- [ ] `check:arquetipos-parity --strict` en CI **o** defer justificado.
- [ ] A11y modal/tabs documentado pass o issue abierto.
- [ ] Hub: F55 archivo; F56 solo si hay carry de remove deprecated.
- [ ] Checks convención verdes.

## Relación con F54

| F54 deja | F55 profundiza |
|----------|----------------|
| Oleada 1 divider/progress/chip | Oleada 2 select/icon/pagination |
| SB native en CI | Chromatic / visual |
| Parity soft + manifest | Strict + smoke Playwright |
| A11y matriz + smoke | Focus trap / tabs teclado |
| Coverage soft | Strict BE (C1) + Jest workspace rollout (C3) |
| Matriz Figma | Code Connect piloto |
| — | Storybook targets arquetipos UI |
