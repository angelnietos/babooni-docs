<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F49-A2 — Limpiar `as any` en implementaciones base-backend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


## Estado

completado (implementaciones); residual en int-specs → F50

## Resultado

- **Implementaciones (no-spec):** `as any` = 0 en `libs/base/backend/src/lib/**/*.ts`
  excluyendo `*.spec.ts` / `*.int-spec.ts`.
- `audit-context.ts` tipado con `http.getRequest<AuthedRequest & …>()` (sin `as any`).
- **Residual:** `as any` en `*.int-spec.ts` / algunos `*.spec.ts` de Prisma delegates
  y messaging — se traslada a **F50-A3**.

## Objetivo (histórico)

Eliminar `as any` residuales en archivos de implementación de `base-backend`.

## Criterios de aceptación

- [x] Cero `as any` en implementaciones de domains (no-spec).
- [ ] Tipado completo de int-specs Prisma — **carry → F50-A3**.
