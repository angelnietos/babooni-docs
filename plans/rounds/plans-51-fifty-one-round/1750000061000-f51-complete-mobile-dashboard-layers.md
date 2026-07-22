<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F51-A2 — Completar capas dashboard mobile (lib-layout)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

Thin re-exports de `@base/*` bajo arquetipos (patrón auth/clients):

**RN:** `@arquetipos/react-native-dashboard-{api,data-access,features}`  
**Ionic:** `@arquetipos/ionic-dashboard-{api,data-access,features}`

- Paths en `tsconfig.base.json` + `check:exports-paths` OK (371 packages).
- `check-lib-layout`: warns dashboard **eliminados** (queda solo storybook josanz, preexistente).
- Tags `layer:arquetipos` + `runtime:react|angular` + `type:api|data-access|feature`.

## Criterios de aceptación

- [x] Warns de dashboard mobile resueltos.
- [x] Tags `runtime:*` / `layer:arquetipos` coherentes.
- [x] Typecheck de capas nuevas / shells (ver Resultado / CI local).
