# F57-A2 — Coverage inventory + CI soft → strict

## Estado

listo para ejecutar

## Objetivo

Tras A1, inventariar **quién tiene tests y quién reporta coverage**, y cablear
el check en CI (soft primero) para que el informe “global” no mienta en
affected/main.

## Entregables

1. Script `tools/jest/report-coverage-inventory.mjs` (o extensión de A1):
   - Lista proyectos Nx con `test` target + jest config.
   - Clasifica: `has-specs` | `passWithNoTests` | `no-jest` | `coverage-ok` | `coverage-missing`.
   - Escribe `coverage/inventory.json` + resumen stdout.
2. `package.json`:
   - `check:coverage-truth` → `node tools/jest/check-coverage-truth.mjs`
   - `check:coverage-truth:strict`
   - `report:coverage-inventory`
3. CI (job `quality` o `frontend`):
   - Tras `test:coverage:affected` + merge (si ya existe): correr `check:coverage-truth` **soft** (`continue-on-error: true`).
   - Criterio para pasar a **strict** en la misma ronda o defer F58: 0 missing en `tag:layer:base` affected en main durante N PRs / o inventario base verde local.
4. Docs: tabla en jest-coverage.md “Cómo leer el informe” (por proyecto vs global vs BE gate).

## Criterios de aceptación

- [ ] Inventario reproducible localmente.
- [ ] Soft gate en CI con artefacto/log legible.
- [ ] Decisión explícita: strict en F57 **o** defer F58 con umbral/motivo.
- [ ] No falla PR por apps sin specs legítimas (`passWithNoTests` allowlisted).

## Verificación

```bash
pnpm test:coverage:report:affected   # o subset
pnpm report:coverage-inventory
pnpm check:coverage-truth
```
