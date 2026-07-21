# Ronda F49 — Completar imports profundos + any-cast cleanup + frontend typecheck

## Estado

completado (parcial — carry-over a F50)

## Progreso

- **F49-A1** completada: cero deep-imports en domains `base-backend`.
- **F49-A2** completada (implementaciones): cero `as any` en no-spec; int-specs → F50.
- **F49-A3** trasladada a F50-A1 (frontend typecheck).
- **F49-A4** trasladada a F50-A2 (tests backend faltantes).
- **F49-B1** completada: docs + apertura F50.

## Hitos

- [x] [F49-A1 — Eliminar imports profundos restantes](1750000040000-f49-eliminate-deep-relative-imports-final.md)
- [x] [F49-A2 — Limpiar `as any` en implementaciones](1750000041000-f49-cleanup-any-casts-in-implementations.md)
- [ ] [F49-A3 — Frontend typecheck](1750000042000-f49-frontend-typecheck-coverage.md) → **F50-A1**
- [ ] [F49-A4 — Tests backend faltantes](1750000043000-f49-add-missing-backend-tests.md) → **F50-A2**
- [x] [F49-B1 — Documentación y cierre](1750000044000-f49-documentation.md)

## Objetivo (cumplido en parte)

1. Completar deuda de imports profundos — **hecho**.
2. Eliminar `as any` en implementaciones — **hecho** (int-specs en F50).
3. Frontend typecheck amplio — **F50**.
4. Tests faltantes dominios — **F50**.
5. Documentación — **hecho** (+ biblia motor).

## Carry-over

Ver [plans-50-fifty-round](../plans-50-fifty-round/README.md).

## Criterios de aceptación (F49)

- [x] Cero `../../../../` en domains no-spec.
- [x] Cero `as any` en implementaciones no-spec.
- [ ] Typecheck FE amplio — F50.
- [ ] Cobertura tests dominios — F50.
- [x] Docs / plans index actualizados.
