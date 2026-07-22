<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F63-A2 — Tenant switcher UX</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Pulir el switcher del header: label que no recorta, trigger denso, teclado,
feedback visual al cambiar tenant (sin recargar a ciegas).

## Entregables

1. Label responsive (ocultar &lt;900px); select min/max width estables.
2. `aria-*` + focus ring con tokens tenant.
3. Opcional: toast/eyebrow “Tema: Helix Labs” 1.2s al cambiar (no modal).

## Criterios de aceptación

- [ ] No se lee “Tenan…” cortado en desktop.
- [ ] Teclado Arrow/Enter/Escape operan el listbox.
- [ ] Cambio de tenant re-pinta shell en &lt;100ms percibido.

## Verificación

Manual multi A/R + smoke.

## Depende de

F63-A1.
