# F62-E1 — Docs polish + hub F62 (+ carry F60-F1)

## Estado

listo para ejecutar

## Objetivo

Cerrar documentación de F62 y el carry [F60-F1](../plans-60-sixty-round/1750000179000-f60-documentation-polish.md):
hubs, biblia, design-system, parity, ui-strategy, ui-styles, runbooks CI
visual / Chromatic defer.

## Entregables

1. Hubs: `docs/plans/README.md`, `docs/README.md` → F62 activa; F60 archivo.
2. Frontend: `design-system.md`, `ui-styles-7-1.md`, `ui-strategy.md`,
   `parity.md`, `arquetipos/README.md` — native-first, temas, select, RN tokens,
   shell, Storybook native primero.
3. README F60 → “cerrada / carry F62” (ya parcialmente); alinear hitos.
4. Checklist visual: temas tenant + “no `<select>`” + RN token level.
5. Si D2 defiere Chromatic: nota en `design-system.md` / `ci-gates.md`.
6. Enlaces rotos 0 en planes F62; actualizar AGENTS/biblia solo si contradicen.

## Criterios de aceptación

- [ ] Ningún hub dice “F60 activa” como única ronda.
- [ ] F60 README en cerrada/carry; F62 listado completo (A3/A4 incluidos).
- [ ] `rg "Ronda activa" docs/plans docs/README.md` apunta a F62.

## Verificación

Revisión manual de hubs + grep ronda activa.

## Depende de

Resto F62 (puede ir en paralelo / al cierre).

## Bloquea

Nada.
