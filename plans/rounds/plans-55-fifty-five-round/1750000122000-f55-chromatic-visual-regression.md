# F55-B1 — Chromatic / regresión visual sobre Storybook native-ui

## Estado

defer F56

## Objetivo

F54-B1 puso `build-storybook base-native-ui` en CI y deferió Chromatic por
falta de token. F55 elige e integra **una** estrategia visual.

## Opciones (elegir una)

| Opción | Pros | Contras |
|--------|------|---------|
| Chromatic | DX Storybook nativo | Coste / secret `CHROMATIC_PROJECT_TOKEN` |
| Loki | Local/CI sin SaaS | Mantenimiento baselines |
| Playwright screenshots de páginas SB | Ya hay PW en monorepo | Menos integración SB |

## Tareas

1. Decisión en Resultado (cuenta/token vs Loki vs PW).
2. Soft/nightly primero (`continue-on-error` o workflow separado).
3. Documentar en design-system § CI Storybook + secretos.
4. No alargar PR budget > timeout documentado.

## Criterios de aceptación

- [x] Estrategia integrada en CI (PR o nightly) **o** defer F56 con motivo.
- [ ] Al menos un baseline/smoke visual reproducible localmente.

## Resultado

Defer Chromatic: no hay `CHROMATIC_PROJECT_TOKEN`. Estrategia elegida =
Playwright Storybook screenshots en F56. `build-storybook` sigue como señal CI
(documentado en [design-system.md](../../../frontend/design-system.md)).
