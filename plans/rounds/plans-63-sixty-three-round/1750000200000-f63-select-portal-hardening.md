<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F63-A1 — Select / overlays portal hardening</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

en progreso (portal implementado; verificar manual + stories)

## Objetivo

Garantizar que `base-select` (y el patrón de overlay) sea visible y usable dentro
del shell arquetipos. F62-A2 usó `position: fixed` en shadow DOM; ancestros con
`backdrop-filter` / `filter` / `transform` crean containing block y el panel
queda fuera de pantalla (chevron gira, lista no aparece).

## Entregables

1. Portal del listbox a `document.body` (+ shadow + styles), posición viewport.
2. Click-outside / Escape / scroll-resize reposition; no cerrar en el click de apertura.
3. Test a11y/source: portal + `base-change`.
4. Nota en `design-system.md` / native-ui README: overlays → portal.

## Criterios de aceptación

- [ ] En `/clients` multi-tenant, abrir Tenant muestra nova/helix/orbit/meridian/lumen.
- [ ] Elegir tenant aplica `data-arq-tenant` y tokens visibles.
- [ ] Panel no queda clipado por stage/topbar.

## Verificación

Manual angular-multi :4203 + `pnpm nx test base-native-ui`.

## Depende de

—

## Bloquea

F63-A2, percepción F63.
