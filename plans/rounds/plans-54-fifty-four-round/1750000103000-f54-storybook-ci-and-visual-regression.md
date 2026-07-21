# F54-B1 — CI `build-storybook` native-ui + regresión visual

## Estado

completado

## Objetivo

F53-B2 añade SB local. F54 lo pone en **CI** y, si es viable, **visual
regression** (Chromatic, Loki, o Playwright screenshots).

## Tareas

1. Job/step frontend: `pnpm nx build-storybook base-native-ui` (+ artifact
   estático opcional).
2. Mantener builds existentes `base-angular-ui` / `base-react-ui`.
3. Evaluar Chromatic (cuenta/token) vs Loki local vs Playwright smoke de
   páginas SB.
4. Soft primero (`continue-on-error`) si flaky; strict en follow-up.
5. Documentar en design-system § CI Storybook + secretos necesarios.
6. Cache Nx del target `build-storybook`.

## Criterios de aceptación

- [ ] `build-storybook base-native-ui` en CI (PR o nightly — documentar cuál).
- [ ] Estrategia visual elegida **o** defer explícito con motivo (coste/token).
- [ ] Sin alargar PR > presupuesto razonable (timeout/parallel documentado).
