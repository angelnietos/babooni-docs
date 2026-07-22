<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F62-B1 — Features users / roles / audit / settings — UI polish</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Elevar users, roles, audit y settings al mismo nivel visual que clients
(`arq-clients*`): tipografía, toolbar, tabla/lista, empty/loading, densidades
coherentes. Hoy se ven vacías, “boilerplate” y desalineadas entre stacks.

## Entregables

1. Páginas BEM en `@base/ui-styles` `pages/`:
   - `arq-users*`, `arq-roles*`, `arq-audit*`, `arq-settings*`
   (o un `arq-crud*` compartido + modificadores si YAGNI lo permite).
2. Reescribir paneles Angular + React para usar esas clases + Native* atoms.
3. Paridad copy/estructura Angular ↔ React (misma jerarquía DOM BEM).
4. Next/Ionic: al menos users o settings alineados si existen; si no, defer
   documentado.

## Criterios de aceptación

- [ ] Users no usa `<select>` ni PageHeader/TableToolbar framework en camino feliz.
- [ ] Densidad y radios consistentes con clients.
- [ ] Empty / loading / error con Native empty-state / spinner / alert.

## Verificación

```bash
pnpm nx test arquetipos-angular-users-features
pnpm nx test arquetipos-react-users-features
pnpm arq:fe:e2e:smoke
```

## Depende de

F62-A2 (select), F62-C2 (native-first) en paralelo.

## Bloquea

F62-C1 (showcase debe enlazar a features reales).
