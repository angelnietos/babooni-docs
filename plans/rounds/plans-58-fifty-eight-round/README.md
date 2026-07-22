# Ronda F58 — Pulido coverage, visual/Figma y DX

## Estado

completado (con carries → F59)

> Cerrada 2026-07-22. Carry de F57 (Chromatic / Code Connect / `@deprecated`) +
> coverage-truth **strict**, limpieza `scope:*`, **e2e de todos los arquetipos FE**,
> y pulido tools/docs. Visual/Figma/remove-deprecated → F59.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F58-A1 | [Coverage-truth strict + allowlists](1750000150000-f58-coverage-truth-strict.md) | completado |
| F58-A2 | [Umbrales / lectura informe por capa](1750000151000-f58-coverage-layer-thresholds.md) | completado |
| F58-B1 | [Chromatic o alternativa visual](1750000152000-f58-chromatic-or-visual-alt.md) | trasladado → F59 |
| F58-B2 | [Figma Code Connect pilot](1750000153000-f58-figma-code-connect-pilot.md) | trasladado → F59 |
| F58-B3 | [Remove framework `@deprecated` primitives](1750000154000-f58-remove-deprecated-primitives.md) | trasladado → F59 |
| F58-C1 | [Purge tags `scope:*` residuales](1750000155000-f58-purge-legacy-scope-tags.md) | completado |
| F58-C2 | [Scaffolds: React SaaS + post-scaffold hooks](1750000156000-f58-scaffolds-saas-react-hooks.md) | completado |
| F58-D1 | [MockServer e2e soft + fixtures expand](1750000157000-f58-mockserver-e2e-soft.md) | completado |
| F58-D2 | [E2E todos los arquetipos FE](1750000159000-f58-arquetipos-e2e-all-apps.md) | completado |
| F58-E1 | [Tools/docs polish + hub](1750000158000-f58-documentation-polish.md) | completado |

## Orden ejecutado

1. **A1** — allowlists + CI strict.
2. **C1** — purge `scope:*` (231) + ESLint + `checkNoScopeTags`.
3. **D2** — CI `react-single` + mf soft + `arq:fe:e2e:smoke`; **D1** mock en paralelo.
4. **B1/B2/B3** — defer F59 (sin token Chromatic/Figma; consumers deprecated).
5. **C2** — SaaS Angular-only + smoke + post-scaffold checklist.
6. **A2** — tabla lectura + `check:coverage-thresholds` soft.
7. **E1** — hub + parity + gates.

## Criterios de aceptación (ronda)

- [x] `check:coverage-truth:strict` verde en CI (quality job).
- [x] B1/B2/B3 defer F59 documentado.
- [x] Sin `scope:*` en project.json (gate `checkNoScopeTags`).
- [x] MockServer: smoke + soft e2e documentado.
- [x] E2E: single+multi Angular/React (+ mf soft) en CI + `arq:fe:e2e:smoke`.
- [x] Hub plans + biblia apuntan a F58 cerrada / F59 siguiente.

## Fuera de alcance

- Migrar Jest → Vitest.
- Umbral % global único para todo el monorepo (A2 es por capa/opt-in).
- Reescribir generadores Nx oficiales.
- Next / Ionic / RN Playwright e2e → **cerrado en F59-C1**.
