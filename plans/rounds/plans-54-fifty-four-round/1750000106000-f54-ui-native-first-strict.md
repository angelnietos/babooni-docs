<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F54-C1 — Gate `ui-native-first` strict + ratchet allowlist</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

F53-C2 introduce soft warn + allowlist legacy. F54:

1. Promueve a **strict** en hygiene/CI cuando soft sea estable.
2. **Ratchet:** la allowlist solo puede menguar (no crecer) salvo excepción
   firmada en Resultado.

## Tareas

1. Auditar warns soft en CI/local post-F53.
2. Quitar entradas allowlist ya migradas (A1).
3. `pnpm check:ui-native-first:strict` en `pnpm hygiene` o job frontend.
4. Actualizar ci-gates + pr-checklist.
5. Si flaky: defer con métrica (igual que coverage).

## Criterios de aceptación

- [ ] Strict en CI **o** defer documentado.
- [ ] Allowlist size ≤ baseline F53 (ratchet).
- [ ] Mensaje de fallo apunta a native-ui + F53-A1/F54-A1.
