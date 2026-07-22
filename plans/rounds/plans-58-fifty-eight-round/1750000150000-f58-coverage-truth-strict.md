# F58-A1 — Coverage-truth strict + allowlists

## Estado

completado

## Objetivo

Pasar `pnpm check:coverage-truth:strict` a gate CI (job `quality`) sin ruido:
marcar configs con inventario intencional y fallar solo en mentiras reales
(`coverage/root`, missing `coverage-final` con specs, 0% sospechoso).

## Entregables

1. Añadir `// coverage-truth: allow-broad-collect` (o allowlist en script) a
   jest configs de apps/backends que usan `collectCoverageFrom` inventario a propósito.
2. CI: cambiar soft → strict (o soft hasta N PRs verdes + strict en main).
3. Documentar en [jest-coverage.md](../../../runbooks/jest-coverage.md).

## Criterios de aceptación

- [x] `pnpm check:coverage-truth:strict` exit 0 en workspace limpio tras
      `test:coverage:affected` + merge (o inventario documentado).
- [x] CI actualizado; `ci-gates.md` refleja strict/soft.
- [x] Sin reintroducir `collectCoverageFrom` global en `jest.shared.cjs`.

## Verificación

```bash
pnpm test:coverage:affected && pnpm test:coverage:merge
pnpm report:coverage-inventory
pnpm check:coverage-truth:strict
```
