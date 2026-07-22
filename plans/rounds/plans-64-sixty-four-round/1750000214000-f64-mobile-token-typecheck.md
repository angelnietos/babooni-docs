<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-B2 — RN/Ionic token + typecheck hygiene</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Eliminar leftovers indigo/teal hardcodeados en mobile y arreglar typecheck de
specs que bloquean `nx typecheck` (p. ej. `home.page.spec.ts` TS2347).

## Entregables

1. `rg "#4f46e5|#6366f1|#14b8a6"` en `libs/base/frontend/mobile` → tokens
   (`arq.*` / `--arq-brand`) o fallback documentado.
2. `ArqAvatar` / mocks RN: paleta desde tokens tenant, no indigo fijo.
3. Specs Ionic clients: tipar helpers Jest (fin TS2347) para
   `base-ionic-clients-features:typecheck` verde.
4. No re-lock `--arq-brand` en `:host` (anti-patrón checklist).

## Criterios de aceptación

- [ ] `pnpm nx typecheck base-ionic-clients-features` verde.
- [ ] Sin indigo hardcode en avatar/features RN camino feliz.
- [ ] Gradientes FAB/hero usan `--arq-brand` / `--arq-cta`.

## Verificación

```bash
pnpm nx typecheck base-ionic-clients-features
pnpm nx typecheck base-react-native-auth-features
rg -n "#4f46e5|#6366f1" libs/base/frontend/mobile
```

## Depende de

—
