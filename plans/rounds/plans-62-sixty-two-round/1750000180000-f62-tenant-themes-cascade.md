<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F62-A1 — Temas tenant distinguibles + cascade</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Que cada tenant demo (Helix Labs, Nova Retail, …) se sienta **claramente
distinto**: no solo un label en el topbar. Hoy el acento, el CTA y el sidebar
mezclan teal/coral/indigo y el cambio de tema casi no se nota.

## Entregables

1. Matriz de temas en `@base/ui-styles` `themes/` (o `arquetipos-theme.scss`):
   por tenant → `--color-brand`, `--color-cta`, `--color-hero-from/to`,
   `--arq-sidebar-active`, `--focus-ring`, superficies.
2. Cascade documentada: `html[data-tenant]` / clase body → vars → Native CE +
   BEM pages + shell.
3. Smoke visual: Angular/React multi — cambiar tenant actualiza botón primary,
   active nav y hero login **sin reload** (si el store ya emite).
4. Eliminar leftovers indigo/coral que pisen el tema activo.

## Criterios de aceptación

- [ ] Al menos 3 tenants con contraste perceptible (hue + CTA + active nav).
- [ ] `base-button` primary y sidebar active usan las mismas vars de tema.
- [ ] Checklist en `docs/frontend/` o parity (cómo verificar a ojo).

## Verificación

```bash
pnpm nx typecheck base-ui-styles
# Manual: angular-multi :4203 — cambiar tenant y comparar CTA / nav
```

## Depende de

Nada (bloquea B2 / percepción F62).

## Bloquea

F62-B2, parcialmente F62-B1.
