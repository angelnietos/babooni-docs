<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F67-A2 — FeatureShell adoption closeout</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F67" src="https://img.shields.io/badge/round-F67-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Pasar de “clases `arq-feature*` en markup” a **adopción real del componente
shell** (Angular/React) y retirar la deuda semántica del alias `arq-clients*`,
sin repetir la rotura de CSS dual (selectores dentro de `@keyframes` /
`@charset` / `var()`).

## Problema actual

- Ionic/pages usan BEM `arq-feature*` pero el chrome aún es HTML+CSS local.
- CSS dual `.arq-feature*, .arq-clients*` se regeneró mal en F66 (warnings
  Angular compiler + error Sass `#0d5f59))`).
- Alias `clients-list` → `feature-list` documentado; retirada no planificada.

## Entregables

### A2.1 — Wire FeatureShell

1. Angular web: ≥1 dominio (clients o roles) monta `FeatureShellComponent`
   (slots header/toolbar/body) en lugar de solo clases CSS.
2. React web: idem con `FeatureShell`.
3. Ionic: documentar si reusa solo CSS o thin wrapper; sin pages
   MobileX/DesktopX duplicadas.

### A2.2 — Alias `arq-clients*`

1. Inventario de consumers de `arq-clients*` / `clients-list`.
2. Plan de retirada: deprecar en docs; opcional remove alias en F67 si
   zero consumers, si no defer F68 con lista.
3. Mantener `pages/feature-list` como SoT en `@base/ui-styles`.

### A2.3 — Dual-CSS safety

1. No dualizar con regex naive sobre bloques enteros.
2. Si se regenera dual: script que solo toque **selectores de clase**
   (no `@charset`, `@keyframes`, ni comas dentro de `linear-gradient`/`var`).
3. Smoke: `pnpm nx test base-ui-styles` + build Angular multi sin
   `css-syntax-error`.

## Criterios de aceptación

- [ ] ≥2 stacks (Angular + React **o** Angular + Ionic) con shell cableado
      o justificación documentada para “CSS-only” en mobile.
- [ ] Inventario alias + decisión retirada / defer F68.
- [ ] Build Angular arquetipos sin warnings `feature-list.css` charset/keyframes.
- [ ] Guía [feature-shell-presentation.md](../../../frontend/feature-shell-presentation.md)
      actualizada (estado adopción).

## Verificación

```bash
pnpm nx test base-ui-styles
pnpm nx typecheck base-angular-clients-features
pnpm nx typecheck base-react-clients-features
# serve ionic-multi /clients — table desktop, cards mobile; sin list+table doble
```

## Depende de

F66-A3; fix CSS dual ya aplicado en `feature-list.css` / `_feature-list.scss`.
