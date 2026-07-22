# F57-A1 — Coverage report truth (por app / lib)

## Estado

completado

## Objetivo

Asegurar que el **reporte de cobertura** (HTML/LCOV/json por proyecto y el
merge `coverage/global/`) refleja la realidad de **cada app y lib** que corre
tests — sin filas fantasma a 0%, sin proyectos con tests que no emiten
`coverage-final.json`, y sin mezclar gates BE de dominio con el merge workspace.

## Contexto (deuda F55/F56)

| Problema | Efecto |
|----------|--------|
| `collectCoverageFrom` amplio en shared (ya corregido) | Paredes 0% (Lit mockeados) |
| Coverage solo vía `nx test` vs `jest -c` | Paths `coverage/root` vs `coverage/<projectRoot>` |
| Merge une lo que encuentra | Puede omitir proyectos o duplicar si hay dirs basura |
| Apps `passWithNoTests` | Inflan “verdes” sin señal de coverage |
| Gate `test:cov:check` (audit/settings/tenants) | Correcto pero **aparte** del merge global — hay que documentar y no confundir |

## Entregables

1. **Auditoría** documentada en [jest-coverage.md](../../../runbooks/jest-coverage.md):
   - Qué cuenta como “proyecto con unit tests” (tiene `jest.config.*` + al menos un `*.spec.ts` / `*.test.ts`, o `passWithNoTests` explícito).
   - Qué **no** debe entrar en el informe (stories, `index.ts` barrels, `*.d.ts`, CEs Lit mockeados si no se ejecutan).
2. Script `tools/jest/check-coverage-truth.mjs`:
   - Input: tras `pnpm test:coverage:affected` o lista de project roots.
   - Verifica por proyecto con tests reales:
     - existe `coverage/<projectRoot>/coverage-final.json` (o json-summary);
     - `coverageDirectory` del config apunta bajo `coverage/<projectRoot>`;
     - no hay `collectCoverageFrom` amplio no justificado (salvo opt-in / BE gate);
     - summary: líneas/statements > 0 **si** hubo specs ejecutados (warn si 0% total con tests).
   - Exit 0 warn / `--strict` fail.
3. Ajustar `merge-jest-coverage.mjs` si hace falta:
   - Ignorar `coverage/root`, dirs vacíos, duplicados.
   - Emitir `coverage/global/projects.json` con lista de proyectos mergeados + path origen.
4. Canarios documentados: ≥1 lib Angular, ≥1 React/UI tokens, ≥1 backend domain (o `base-backend`), ≥1 app smoke si aplica.

## Criterios de aceptación

- [x] `pnpm nx test <lib> -- --coverage` → artefactos solo bajo `coverage/<projectRoot>/`.
- [x] `check-coverage-truth` detecta al menos: missing coverage-final, wrong coverageDirectory, broad collectCoverageFrom no allowlisted.
- [x] Merge produce `projects.json` auditable; runbook actualizado (pitfalls + comandos).
- [x] Gate BE `test:cov:check` **sigue** siendo la fuente de umbrales dominio; no se diluye en el merge global.
- [x] Sin reintroducir `collectCoverageFrom` global en `jest.shared.cjs`.

## Verificación

```bash
pnpm nx test base-ui-tokens -- --coverage
pnpm nx test base-backend -- --coverage   # o dominio concreto
node tools/jest/check-coverage-truth.mjs --roots libs/base/frontend/crosscutting/ui-tokens
pnpm test:coverage:merge
test -f coverage/global/projects.json   # o equivalente Windows
```

## Enlaces

- [jest-coverage.md](../../../runbooks/jest-coverage.md)
- [F55-C3](../plans-55-fifty-five-round/1750000126500-f55-jest-workspace-coverage-rollout.md)
- [F56-B1](../plans-56-fifty-six-round/1750000132000-f56-backend-jest-coverage-extend.md)
