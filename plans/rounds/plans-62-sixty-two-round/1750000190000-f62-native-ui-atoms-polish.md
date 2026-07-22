<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F62-A3 — Polish átomos `@base/native-ui`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Cerrar [F60-A2](../plans-60-sixty-round/1750000171000-f60-native-ui-atoms-polish.md):
aplicar brief cinematic HUD / tokens a **todos** los Custom Elements Lit SoT
para que wrappers DOM hereden el look sin CSS forked en features.

Hoy button/input/alert están “ok pero flat”; select es peor (→ A2); densidades y
focus no son coherentes entre átomos.

## Átomos canario (mínimo)

| CE | Notas |
|----|--------|
| `base-button` | primary/secondary/ghost/danger; sizes; loading; focus-visible; CTA = vars tema |
| `base-input` | label, error, disabled; densidades |
| `base-select` | listbox (detalle en A2; aquí integración visual) |
| `base-alert` | tones |
| `base-card` | elevated / flat |
| `base-badge` / `base-chip` | densidad |
| `base-spinner` / `base-avatar` / `base-empty-state` | coherencia de tamaño |
| `base-table` (si existe) | header/row hover alineado a `arq-clients` |

Path: [`libs/base/frontend/crosscutting/native-ui`](../../../../libs/base/frontend/crosscutting/native-ui).

## Entregables

1. Estilos CE alineados a `@base/ui-tokens` + cascade tema (A1).
2. Sin nuevos primitivos framework-only (ADR 0010 freeze).
3. Checklist a11y: contraste, focus ring, `aria-busy` en loading.
4. Smoke visual: login + clients + `/native-ui` Angular/React.

## Criterios de aceptación

- [ ] Átomos canario actualizados; primary button cambia con tenant (A1).
- [ ] `pnpm nx test base-native-ui` + `typecheck` verdes.
- [ ] No regresiones `registerNativeUi()`.

## Verificación

```bash
pnpm nx test base-native-ui
pnpm nx typecheck base-native-ui
```

## Depende de

F62-A1 (temas); F62-A2 en paralelo para select.

## Bloquea

F62-C1 (showcase), F62-D3 (stories), parcialmente B1/B2.
