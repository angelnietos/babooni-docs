# F51-C2 — Higiene artefactos + ignore `tsbuildinfo`

## Estado

listo para ejecutar

## Objetivo

Evitar que artefactos de typecheck (`*.tsbuildinfo`) y similares entren en
commits / PRs. Alinear `.gitignore` y limpiar tracked si aplica.

## Tareas

1. Auditar `git ls-files '*.tsbuildinfo'`.
2. Añadir / confirmar ignore en `.gitignore`.
3. `git rm --cached` de tracked sin borrar el archivo local si es útil.
4. Opcional: script pre-commit / check en CI que falle si se stagean.

## Criterios de aceptación

- [ ] Ningún `*.tsbuildinfo` tracked (salvo excepción documentada).
- [ ] `.gitignore` cubre el patrón.
