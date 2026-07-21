# F56-A2 — Visual QA apps arquetipos (“se ven bien”)

## Estado

completado

## Objetivo

Verificar que las SPAs plantilla sirven y se ven bien en el flujo canario,
preferiblemente con MockServer (sin API/Keycloak).

## Criterios de aceptación

- [x] Checklist o PW soft reproducible con mock.
- [x] Misma secuencia Angular ↔ React documentada.
- [x] Sin exigir Chromatic (D1).

## Resultado

Checklist en [mockserver.md](../../../runbooks/mockserver.md) y
[parity.md](../../../arquetipos/parity.md). E2E `e2e-web` en main sigue con
Keycloak real; modo mock es el camino diario FE-only.
