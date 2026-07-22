<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F46 — Typecheck hygiene + DX improvements</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Hitos

- [F46-A1 — Corregir imports rotos en `base-shared`](1750000010000-f46-fix-shared-typecheck.md)
- [F46-A2 — Ajustar tsconfig de `base-react-ui` para excluir stories de spec](1750000011000-f46-fix-react-ui-tsconfig.md)
- [F46-A3 — Corregir mismatch de tipos React en `ArqTabs`](1750000012000-f46-fix-react-native-types.md)
- [F46-A4 — Corregir facade `josanz-clients-data-access`](1750000013000-f46-fix-josanz-clients-facade.md)
- [F46-B1 — Corregir extends de tsconfig en libs Ionic](1750000014000-f46-fix-ionic-tsconfig-paths.md)
- [F46-C1 — Documentación y cierre de ronda](1750000015000-f46-documentation.md)

## Objetivo

1. **Typecheck hygiene**: eliminar fallos preexistentes de `tsc --noEmit` que
   bloquean la validación completa del workspace.
2. **DX improvements**: ajustar configuraciones de tsconfig que generan ruido
   en CI/IDE.
3. **Facade correctness**: alinear la facade Josanz con la API pública del
   servicio base.
4. **Documentación**: mantener índices y guías alineados con el estado real.

## Orden de ejecución

1. F46-A1 (base-shared imports) → depende de: nada.
2. F46-A2 (base-react-ui tsconfig) → depende de: nada.
3. F46-A3 (ArqTabs types) → depende de: nada.
4. F46-A4 (josanz facade) → depende de: nada.
5. F46-B1 (Ionic tsconfig paths) → depende de: nada.
6. F46-C1 (documentación) → depende de: F46-A1, F46-A2, F46-A3, F46-A4, F46-B1.

## Riesgos

- **Cambios en tsconfig**: mitigado por validar con `pnpm nx typecheck` por proyecto.
- **Cambios en facade**: mitigado por revisar call sites antes de modificar.
- **React type mismatch**: mitigado por usar cast explícito localizado.

## Criterios de aceptación

- [x] `pnpm check:deprecated` pasa.
- [x] `pnpm check:exports-paths` pasa.
- [x] `pnpm check:node-nx` pasa en CI (Node 22).
- [x] Los proyectos `base-shared`, `base-react-ui`, `base-react-native-ui`, `josanz-clients-data-access` pasan `typecheck`.
- [x] Guías actualizadas en `docs/guides/`.
