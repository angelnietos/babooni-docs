# Ronda F58 — Pulido coverage, visual/Figma y DX

## Estado

listo para ejecutar

> Abierta 2026-07-22. Carry de F57 (Chromatic / Code Connect / `@deprecated`) +
> coverage-truth **strict**, limpieza `scope:*` residual y pulido tools/docs.

## Hitos

| ID | Plan | Estado |
|----|------|--------|
| F58-A1 | [Coverage-truth strict + allowlists](1750000150000-f58-coverage-truth-strict.md) | listo para ejecutar |
| F58-A2 | [Umbrales / lectura informe por capa](1750000151000-f58-coverage-layer-thresholds.md) | listo para ejecutar |
| F58-B1 | [Chromatic o alternativa visual](1750000152000-f58-chromatic-or-visual-alt.md) | listo para ejecutar |
| F58-B2 | [Figma Code Connect pilot](1750000153000-f58-figma-code-connect-pilot.md) | listo para ejecutar |
| F58-B3 | [Remove framework `@deprecated` primitives](1750000154000-f58-remove-deprecated-primitives.md) | listo para ejecutar |
| F58-C1 | [Purge tags `scope:*` residuales](1750000155000-f58-purge-legacy-scope-tags.md) | listo para ejecutar |
| F58-C2 | [Scaffolds: React SaaS + post-scaffold hooks](1750000156000-f58-scaffolds-saas-react-hooks.md) | listo para ejecutar |
| F58-D1 | [MockServer e2e soft + fixtures expand](1750000157000-f58-mockserver-e2e-soft.md) | listo para ejecutar |
| F58-E1 | [Tools/docs polish + hub](1750000158000-f58-documentation-polish.md) | listo para ejecutar |

## Orden sugerido

1. **A1** (coverage strict) — cierra el soft de F57.
2. **C1** (purge scope tags) — hygiene rápido, desbloquea check-lib-layout limpio.
3. **B1/B2/B3** — solo si hay token Chromatic / Figma; si no, defer F59 con motivo.
4. **C2 / D1** — scaffolds + mock e2e en paralelo.
5. **A2** — umbrales por capa solo tras A1 estable.
6. **E1** — cierre docs.

## Criterios de aceptación (ronda)

- [ ] `check:coverage-truth:strict` verde en CI (o defer F59 con allowlist acotada).
- [ ] B1/B2/B3 hechos **o** defer F59 documentado.
- [ ] Sin `scope:*` en project.json nuevos ni en libs arquetipos React/Angular tocadas.
- [ ] MockServer: smoke + opcional soft e2e documentado.
- [ ] Hub plans + biblia apuntan a F58 activa / F59 siguiente.

## Fuera de alcance

- Migrar Jest → Vitest.
- Umbral % global único para todo el monorepo (A2 es por capa/opt-in).
- Reescribir generadores Nx oficiales.
