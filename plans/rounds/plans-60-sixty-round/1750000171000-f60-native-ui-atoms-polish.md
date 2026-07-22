<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F60-A2 — Polish átomos `@base/native-ui`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Aplicar el brief F60-A1 a los Custom Elements Lit SoT para que **todos** los
wrappers DOM (Angular/React/Next/Ionic) hereden el mismo look sin forking CSS
en features.

## Átomos canario (mínimo)

| CE | Notas |
|----|--------|
| `base-button` | primary/secondary/ghost/danger; sizes; loading; focus-visible |
| `base-input` | label, error, disabled |
| `base-alert` | tones |
| `base-card` | elevated / flat |
| `base-badge` / `base-chip` | densidad |
| `base-spinner` / `base-avatar` | coherencia de tamaño |

Path: [`libs/base/frontend/crosscutting/native-ui`](../../../../libs/base/frontend/crosscutting/native-ui).

## Entregables

1. Estilos CE alineados a tokens A1 (Shadow DOM + CSS vars cascade).
2. Sin nuevos primitivos framework-only en `@base/angular-ui` / `@base/react-ui`
   (ADR 0010 freeze).
3. Checklist a11y: contraste, focus ring, `aria-busy` en loading.
4. Nota: el bug Shadow DOM `type=submit` ya se mitiga en forms con
   `clicked`/`onClick` — documentar en guía de uso del botón.

## Criterios de aceptación

- [ ] Átomos canario actualizados; smoke visual en Angular + React SPA login.
- [ ] Tests unitarios CE existentes siguen verdes.
- [ ] No regresiones de registro `registerNativeUi()`.

## Verificación

```bash
pnpm nx test base-native-ui   # o nombre Nx del proyecto
pnpm nx typecheck base-native-ui
```

## Depende de

F60-A1.

## Bloquea

F60-B1, F60-C1.
