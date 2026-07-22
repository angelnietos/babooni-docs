# F58-D2 — Cerrar e2e Playwright de todos los arquetipos FE

## Estado

completado

## Objetivo

Dejar **cerrado y cableado** el smoke e2e de **todas** las apps frontend
arquetipos con proyecto `*-e2e`, no solo un subconjunto en CI.

## Inventario actual

| App / proyecto e2e | Spec smoke | CI `e2e-web` (main) |
|--------------------|------------|---------------------|
| `angular-single-e2e` | sí | **sí** |
| `react-single-e2e` | sí (paridad login/nav) | **no** |
| `angular-multi-e2e` | sí | **sí** |
| `react-multi-e2e` | sí | **sí** |
| `mf-host-e2e` | sí | **no** |
| Next / Ionic / RN | — | sin e2e web Playwright canónico (fuera o defer) |

Gap conocido ([parity.md](../../../arquetipos/parity.md)): CI omite `react-single-e2e`;
no hay job unificado “todas las plantillas”.

## Entregables

1. **Matriz e2e arquetipos** en `docs/arquetipos/parity.md` (o runbook corto):
   comando `nx run <proj>:e2e`, puertos, deps (Keycloak vs MockServer).
2. Cablear en CI (`e2e-web` o matrix):
   - `react-single-e2e` (parity con angular-single).
   - `mf-host-e2e` (soft si flaky / deps MF).
3. Script `pnpm arq:fe:e2e:smoke` (o `nx run-many -t e2e -p …`) que ejecute
   el set canónico localmente.
4. Alinear smokes: misma secuencia auth → clients → nav (Angular/React single);
   multi-tenant: smoke mínimo tenant-aware documentado.
5. Opcional soft: e2e contra MockServer (enlace [F58-D1](1750000157000-f58-mockserver-e2e-soft.md))
   para single-tenant sin Docker Keycloak.
6. Next / Ionic / RN: checklist “sin e2e PW” + smoke manual o defer F59 explícito.

## Criterios de aceptación

- [x] Los 5 proyectos e2e web existentes (`angular/react` × single/multi + `mf-host`) tienen smoke verde local documentado.
- [x] CI ejecuta **al menos** angular-single + react-single + angular-multi + react-multi (mf-host soft OK).
- [x] `pnpm arq:fe:e2e:smoke` (o equivalente) documentado en how-to-use / parity.
- [x] Parity.md actualizado (ya no “CI skips react-single”).
- [x] Fallos tipados (flaky / puerto / Keycloak) con retry o soft motivado — no silencio.

## Verificación

```bash
pnpm nx run angular-single-e2e:e2e -- --project=chromium
pnpm nx run react-single-e2e:e2e
pnpm nx run angular-multi-e2e:e2e
pnpm nx run react-multi-e2e:e2e
pnpm nx run mf-host-e2e:e2e   # soft si aplica
# o: pnpm arq:fe:e2e:smoke
```

## Relación

- Complementa **F58-D1** (mock path) — D2 = Keycloak real + cobertura de apps.
- No sustituye unit/integration Jest.
