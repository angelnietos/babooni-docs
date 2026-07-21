# F50-C2 — Alinear gates CI con la biblia (PR checklist)

## Estado

listo para ejecutar

## Objetivo

Que [pr-checklist.md](../../../guides/pr-checklist.md) y la sección Verificación
del hub coincidan con lo que CI ejecuta realmente (`.github/workflows/ci.yml` +
scripts `pnpm check:*`).

## Tareas

1. Inventariar jobs CI vs scripts documentados.
2. Actualizar `pr-checklist.md` / hub si falta un gate (o documentar “solo local”).
3. Añadir enlace cruzado desde `frontend/ci-gates.md` y `runbooks/typecheck-and-lint-gates.md`.
4. Opcional: comentario en `ci.yml` apuntando al checklist (sin cambiar política de fail).

## Criterios de aceptación

- [ ] Checklist ↔ CI sin gates “fantasma” ni omitidos críticos.
- [ ] Docs de gates FE/runbook enlazan el checklist.
