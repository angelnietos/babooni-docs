# Ronda F54 — Madurez native-ui, migración masiva, visual CI e imports

## Estado

completado

## Resultado (2026-07-22)

| ID | Entrega |
|----|---------|
| A1 | Features arquetipos auth/clients/roles/users → `ArqNative*` (Angular+React). Allowlist native-first vacía. |
| A2 | Inventario + oleada 1: `base-divider`, `base-progress`, `base-chip` + wrappers. Residual select/icon → F55. |
| A3 | `@deprecated` en Button/Alert/Input base Angular+React; remove no antes F56. |
| B1 | `build-storybook base-native-ui` en CI; Chromatic defer (sin token). |
| B2 | `@base/ui-tokens` (+ css/rn); RN theme re-export. |
| B3 | Matriz a11y + `native-ui-a11y.spec.ts` (button/input/modal). |
| B4 | `parity.manifest.yaml` + `check:arquetipos-parity` + `docs/arquetipos/parity.md`. Visual Playwright defer. |
| C1 | `check:ui-native-first[:strict]` en hygiene + CI; allowlist ratchet vacía. |
| C2 | Oleada publishable: `@base/react-ui` + `@base/ui-tokens` (tag + version 0.1.0). |
| D1 | Matriz Figma↔CE en `figma-native-ui-map.md` (Code Connect defer). |
| D2 | Coverage BE strict **defer F55** (sigue soft). |
| E1 | Hub/docs sync. |

También: wrappers Native* para los 14 CEs previos (cierre F53-A2) + Storybook native (F53-B2).

## Carry → F55

Ronda abierta: [plans-55-fifty-five-round](../plans-55-fifty-five-round/) —
oleada Lit 2 (select/icon/pagination), Chromatic, parity/coverage strict,
Code Connect, a11y gaps, Storybook arquetipos. Remove `@deprecated` → F56.
