# Ronda F55 — Carry F54: oleadas Lit, visual CI, coverage strict, Code Connect

## Estado

completado

> Cerrada 2026-07-22. Carry residual (Chromatic token, Code Connect Figma,
> remove deprecated) → **[F56-D1](../plans-56-fifty-six-round/1750000135000-f56-carry-chromatic-code-connect-deprecated.md)**;
> build/visual apps + MockServer → ronda **[F56](../plans-56-fifty-six-round/)**.

## Dependencias externas

- F54 completada (migración P0, oleada 1 Lit, `@base/ui-tokens`, parity soft).
- Token Chromatic / Figma Code Connect — **defer F56** (documentado).
- Coverage BE: `pnpm test:cov:check` verde local → **strict** en CI.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F55-A1 | [Oleada 2 Lit: select / icon / pagination](1750000120000-f55-lit-wave-select-icon-pagination.md) | **completado** |
| F55-A2 | [Filter bar + residual inventario](1750000121000-f55-lit-wave-filter-bar-residual.md) | **completado** (reject CE) |
| F55-B1 | [Chromatic / visual regression](1750000122000-f55-chromatic-visual-regression.md) | **defer F56** (sin token) |
| F55-B2 | [Parity visual smoke Playwright](1750000123000-f55-arquetipos-parity-visual-smoke.md) | **completado** (secuencia doc + smoke specs) |
| F55-B3 | [A11y gaps modal/tabs](1750000124000-f55-native-ui-a11y-gaps.md) | **completado** |
| F55-C1 | [Coverage BE soft → strict](1750000125000-f55-coverage-ci-strict.md) | **completado** |
| F55-C2 | [`check:arquetipos-parity --strict`](1750000126000-f55-arquetipos-parity-strict.md) | **completado** |
| F55-C3 | [Jest workspace preset + coverage](1750000126500-f55-jest-workspace-coverage-rollout.md) | **completado** (~100% preset) |
| F55-D1 | [Figma Code Connect piloto](1750000127000-f55-figma-code-connect-pilot.md) | **defer F56** (sin token) |
| F55-D2 | [Storybook serve arquetipos UI](1750000128000-f55-arquetipos-storybook-targets.md) | **completado** (previo) |
| F55-E1 | [Documentación y cierre](1750000129000-f55-documentation.md) | **completado** |

## Resultado (ronda)

1. Oleada 2: `base-select` / `base-icon` / `base-pagination` + wrappers Native*/ArqNative* + stories.
2. Filter bar: reject CE → composición NativeInput + NativeChip.
3. A11y: modal focus trap + tabs/segment arrow keys; matriz actualizada.
4. CI: `test:cov:check` **strict**; `check:arquetipos-parity --strict`; Jest preset adoption soft + coverage merge soft.
5. Chromatic / Code Connect: defer F56 con motivo (sin secretos).
6. Parity smoke: misma secuencia angular/react documentada en `docs/arquetipos/parity.md`.

## Criterios de aceptación (ronda)

- [x] Oleada 2 con ≥2 CEs nuevos + wrappers.
- [x] Estrategia visual elegida **o** defer con motivo (PW screenshots futuro / build-storybook ahora).
- [x] Coverage BE strict.
- [x] Jest shared preset ≥70% (100% shared+excluded).
- [x] `check:arquetipos-parity --strict` en CI.
- [x] A11y modal/tabs pass.
- [x] Hub F55 archivo; F56 carry Chromatic/Code Connect/remove deprecated.
- [x] Checks convención (parity, jest-preset, cov) verdes localmente.
