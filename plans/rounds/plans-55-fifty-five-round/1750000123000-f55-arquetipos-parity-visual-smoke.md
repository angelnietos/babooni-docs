# F55-B2 — Smoke Playwright paridad Angular↔React (arquetipos)

## Estado

listo para ejecutar

## Objetivo

F54-B4 midió arquitectura + UI imports. Falta señal de **layout de página**
entre stacks Full (mismo flujo canario: auth → clients).

## Tareas

1. Spec Playwright soft: angular-single vs react-single (login demo + list
   clients empty/error/loading si estable).
2. Artifact screenshots opcional; no bloquear A1.
3. Enlazar desde `docs/arquetipos/parity.md`.
4. Si flaky: soft + issue; no fallar PR hasta estable.

## Criterios de aceptación

- [ ] Spec soft en CI **o** defer justificado (“native-ui suficiente”).
- [ ] Misma secuencia documentada en parity.md.
