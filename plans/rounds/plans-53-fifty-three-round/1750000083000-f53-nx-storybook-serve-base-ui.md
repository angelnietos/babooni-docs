<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F53-B1 — Nx `storybook` (serve) en UI base Angular / React</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Hoy:

| Proyecto | `build-storybook` | `storybook` (serve) |
|----------|-------------------|---------------------|
| `josanz-angular-ui` | sí | sí (:4401) |
| `base-angular-ui` | sí | **no** |
| `base-react-ui` | sí | **no** |

Desarrollar UI base obliga a build estático o copiar el patrón Josanz a mano.
Añadir target **`storybook`** (serve) simétrico a Josanz.

## Tareas

1. `base-angular-ui`: target `storybook` con `@storybook/angular:start-storybook`
   (puerto libre, p. ej. **4402**), reutilizando `.storybook` existente.
2. `base-react-ui`: target `storybook` (Vite/React) alineado al build actual
   (puerto p. ej. **4403**).
3. Scripts root opcionales: `pnpm storybook:base-angular`, `storybook:base-react`.
4. Documentar en [design-system.md](../../../frontend/design-system.md) § Storybook.
5. Smoke: `pnpm nx storybook base-angular-ui` / `base-react-ui` arranca (no CI).

## Criterios de aceptación

- [ ] `nx storybook base-angular-ui` y `nx storybook base-react-ui` existen y
      documentados.
- [ ] `build-storybook` sigue verde (CI sin regresión).
- [ ] Puertos no colisionan con Josanz 4401 ni entre sí.
