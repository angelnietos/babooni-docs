<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-C1 — Carry: Storybook SoT + adapters</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Ejecutar carry [F63-D3](../plans-63-sixty-three-round/1750000207000-f63-carry-storybook-atoms.md) /
[F62-D3](../plans-62-sixty-two-round/1750000188000-f62-carry-storybook-atoms.md)
(ADR 0011: Lit primero).

## Entregables

1. Stories canario en `base-native-ui`: button, input, **select+portal**, alert,
   card, checkbox/radio si B3 aterriza.
2. `pnpm nx run base-native-ui:build-storybook` verde.
3. Orphans `libs/arquetipos/frontend/{angular,react}/ui/.storybook/` → cablear
   target Nx **o** borrar + nota en design-system.
4. Adapters brand: solo donde hay delta visual; sin re-story completo Lit.

## Criterios de aceptación

- [ ] `build-storybook` base-native-ui verde.
- [ ] Select portal story documentada.
- [ ] Hecho **o** defer F65 con motivo (capacidad / ADR).

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
pnpm nx run base-angular-ui:build-storybook
pnpm nx run base-react-ui:build-storybook
```

## Depende de

F64-A1.
