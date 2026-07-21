# F53-D1 — Carry: coverage BE soft → strict

## Estado

listo para ejecutar

## Objetivo

F52-B2 dejó `test:cov:check` en soft (`continue-on-error` + artifact). Si en
main el soft **no** dispara warn de forma recurrente, promover a **strict**
(fail job `quality`).

Si aún es flaky: documentar extensión de ventana y no forzar.

## Tareas

1. Revisar últimos CI runs / artifacts `base-backend-coverage`.
2. Si estable: quitar `continue-on-error`, mantener artifact.
3. Actualizar testing-pyramid + ci-gates + pr-checklist.
4. Si inestable: Resultado “defer F54” + motivo.

## Criterios de aceptación

- [ ] Strict en CI **o** defer documentado con evidencia.
- [ ] Umbral sigue alineado F51-A3 (80% / 65%).
