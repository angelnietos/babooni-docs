<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F47-B1 — Limpiar casts `as any` en specs backend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Eliminar los casts `as any` residuales en specs de controllers, repos y services
de `base-backend` que quedaron como workaround temporal y ahora pueden tiparse
correctamente tras la migración a harness.

## Tareas

1. Ejecutar `rg "as any" libs/base/backend/src/lib/domains -l -g "*.spec.ts"` para
   listar archivos afectados.
2. Por cada archivo:
   - Si es un controller spec ya migrado a harness, eliminar el cast.
   - Si es un repo spec ya migrado, eliminar el cast.
   - Si es un service spec, tipar el mock o el resultado con el DTO correcto.
3. Ejecutar `pnpm nx test base-backend` tras cada batch.
4. Ejecutar `pnpm nx typecheck base-backend` al final.

## Criterios de aceptación

- [ ] Cero `as any` en specs de controllers/repos/services en `base-backend`.
- [ ] `pnpm nx test base-backend` verde.
- [ ] `pnpm nx typecheck base-backend` pasa.
