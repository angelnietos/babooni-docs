# F56-A2 — Visual QA apps arquetipos (“se ven bien”)

## Estado

listo para ejecutar

## Objetivo

Además de compilar: verificar que las SPAs plantilla **sirven y se ven bien**
en el flujo canario (auth → clients → users/audit). Preferir modo **MockServer**
(C1/C2) para no depender de API/Keycloak en el día a día del front.

## Prerrequisitos

- Ideal: F56-C1 + C2 (mock). Si mock no listo: checklist manual + API real
  (documentar flaky).
- Smoke Playwright existente en `*-e2e` (F55-B2).

## Tareas

1. Definir checklist visual mínima (viewport desktop):
   - Login demo visible / branding no roto.
   - Tras login: shell (nav) + `main` en `/clients`.
   - Empty/loading/error no pantallas en blanco.
2. Spec Playwright **soft** (o extensión de `smoke.spec.ts`) con screenshots
   opcionales bajo `apps/.../e2e/artifacts/` (gitignored).
3. Preferir `USE_MOCKSERVER=1` + webServer que levante FE + mock (sin gateway).
4. CI: soft en PR **o** solo main/`e2e-web` — no flaky-block.
5. Enlazar desde `docs/arquetipos/parity.md` § visual.

## Criterios de aceptación

- [ ] Checklist o PW soft reproducible con mock **o** defer justificado.
- [ ] Misma secuencia Angular ↔ React documentada.
- [ ] Sin exigir Chromatic (eso es D1).
