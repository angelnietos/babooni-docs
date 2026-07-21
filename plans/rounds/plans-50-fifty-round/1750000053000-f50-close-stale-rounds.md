# F50-B1 — Cerrar rondas zombie (F47) y alinear estados

## Estado

listo para ejecutar

## Objetivo

F47 sigue como «listo para ejecutar» / «en progreso» en índices aunque F48/F49
absorbieron su trabajo (harness, deep-imports, josanz-ui typecheck, any-cast specs).
Cerrar F47 como **superseded/completado por F48–F49** y limpiar `docs/plans/README.md`.

## Tareas

1. Actualizar `plans-47-forty-seven-round/README.md`: estado `completado (superseded por F48/F49)`, hitos tachados con nota de dónde se cerró cada uno.
2. Revisar F43 si aún dice «activa» sin sentido → archivo.
3. Alinear tablas en `docs/plans/README.md` (solo F50 activa; F49 en completadas).
4. No borrar planes históricos.

## Criterios de aceptación

- [ ] F47 no aparece como ronda activa.
- [ ] Cada hito F47 apunta a ronda que lo cerró (F48/F49) o queda N/A.
- [ ] `docs/plans/README.md` coherente.
