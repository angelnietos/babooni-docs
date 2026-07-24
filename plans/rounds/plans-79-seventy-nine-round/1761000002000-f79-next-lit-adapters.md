<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F79-B1 — Next.js Lit adapters (`@base/next-ui`)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F79" src="https://img.shields.io/badge/round-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

Hecho · 2026-07-24 · Depende de: [F79-A1](1761000001000-f79-atom-matrix-and-migration-contract.md)

## Objetivo

Hacer que los átomos de `@base/next-ui` consuman **Lit SoT** vía client islands / wrappers React existentes, eliminando forks CSS como fuente visual.

## Paths

- Lib: `libs/base/frontend/next/ui/` (`base-next-ui`, `@base/next-ui`)
- Referencia wrappers: `libs/base/frontend/react/platform/ui/src/lib/wrappers/native/` (`@base/react-ui`)
- SoT: `libs/base/frontend/crosscutting/native-ui/` (`@base/native-ui`)
- SSR notes: demos / `NativeUiDemo` si existen en apps Next arquetipos

## Problema

`ArqButton`, `ArqInput`, `ArqAlert`, `ArqBadge`, `ArqCard`, … están implementados como markup+CSS propio. Eso rompe paridad con Angular/React y con Storybook Lit.

Next App Router no puede ejecutar Lit en Server Components → hace falta `'use client'` + registro CE (o re-export de `Native*` de `@base/react-ui` desde un barrel next-safe).

## Estrategia

1. Preferir **re-export / thin wrap** de `Native*` (`@base/react-ui`) desde `@base/next-ui` con `'use client'` en el entry del átomo.
2. Si el API público Next (`ArqButton` props) difiere, adaptar con shim de props → `NativeButton` (sin CSS fork).
3. SSR: placeholder semántico opcional solo si hay FOUC crítico; no mantener segundo diseño.
4. Composites (`ArqPageHeader`, `ArqTable`, …) → recomponer con átomos migrados; no migrar chrome Next a Lit.

### Oleadas sugeridas

| Wave | Átomos | Notas |
|------|--------|-------|
| W1 | Button, Input, Select, Icon | forms críticos |
| W2 | Alert, Badge, Chip, Toast, EmptyState | feedback |
| W3 | Card, Divider, Avatar, Skeleton, Spinner, Progress | layout atoms |
| W4 | Checkbox, Radio, Toggle, Segment, Tabs, Modal, Pagination | interactive |
| W5 | Board (+ gaps matriz) | last |

## Acciones

- [ ] Por cada átomo W1–W5: implementar wrapper Lit; tests smoke; marcar CSS legacy `@deprecated`
- [ ] Actualizar `src/index.ts` exports (API estable `Arq*`)
- [ ] Verificar build app canario Next arquetipos (`next-single` / `next-multi`)
- [ ] Actualizar stories Next (ver F79-C1) al mismo tiempo que cada wave
- [ ] Documentar patrón SSR en `docs/frontend/next-native-ui.md` (corto) o sección design-system

## Criterios de aceptación

- [ ] Ningún átomo SoT de la matriz queda como CSS fork sin deprecation path
- [ ] `pnpm nx build` app Next arquetipos verde
- [ ] `pnpm nx typecheck base-next-ui` verde
- [ ] Visual: mismos tokens/`--arq-*` que Lit (inspección Storybook)

## Verificación

```bash
pnpm nx typecheck base-next-ui
pnpm nx build next-single   # o nombre Nx canario
pnpm storybook:base-next
```

## Fuera de alcance

- Migrar todo el App Router a CE raw sin wrappers
- Chromatic (opcional F79-E1 / F80)
