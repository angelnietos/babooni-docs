<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F62-C1 — `/native-ui` = catálogo producto real</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Reemplazar la página “Piloto F35 — Custom Elements…” por un **showcase del
sistema real**: átomos en uso + composiciones (`arq-auth`, `arq-clients`, …) +
enlace a Storybook. El copy debe decir que **el camino feliz de arquetipos es
nativo**; los componentes framework son legado / producto, no el demo.

## Entregables

1. Nueva composición `arq-native-showcase*` (page CSS en ui-styles).
2. Secciones: Buttons, Inputs, Select, Alert, Card, Table densidades, Badge,
   Spinner, Empty — todos Native*.
3. Mini composición “Clients toolbar” / “Login field” embebida (no iframe).
4. Angular + React (y Next si tiene ruta) con el mismo markup BEM.
5. Eliminar copy “piloto”, “wrappers de prueba”, “siguen disponibles en el resto”.

## Criterios de aceptación

- [ ] Ninguna mención a F35 / “piloto Lit” en la UI.
- [ ] Select demo usa listbox nativo (A2).
- [ ] Primary button respeta tema tenant (A1).

## Verificación

Abrir `/native-ui` en angular-multi y react-single; screenshot opcional.

## Depende de

F62-A2, F62-B1 (parcial).

## Bloquea

F62-E1 (docs enlazan showcase).
