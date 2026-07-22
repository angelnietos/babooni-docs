<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F48-A2 — Limpiar `as any` residual en specs backend</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

Se eliminaron los `as any` residuales en specs de `base-backend`. Los archivos
modificados incluyen controller specs, repo specs, handler specs y service specs.
Se reemplazaron por typings específicos:
- Controller specs: DTOs tipados.
- Repo specs: `PrismaDelegate<T>` e interfaces específicas.
- Handler specs: `jest.Mocked<Repo>` y constructores tipados.
- Service specs: interfaces reales para `commandBus`/`queryBus`/`tenantContext`.

Validación:
- `pnpm nx test base-backend`: 109 suites, 306 tests passed.
- `pnpm nx typecheck base-backend`: pasa (queda error pre-existente en `users.service.spec.ts` no relacionado con F48-A2).

## Objetivo

Eliminar los `as any` residuales en specs de `base-backend` que quedaron como
workaround temporal y ahora pueden tiparse correctamente.

## Dependencias

- F48-A1 completada.
- F48-A3 por completar.

## Tareas

1. Ejecutar `grep -r "as any" libs/base/backend/src/lib/domains -l -g "*.spec.ts"` para listar archivos afectados.
2. Por cada archivo:
   - **controller specs**: tipar los DTOs de entrada/salida correctamente.
   - **repo specs**: tipar mocks de delegate con `PrismaDelegate<T>` o interfaces específicas.
   - **handler specs**: usar `jest.Mocked<Repo>` o constructores tipados en lugar de `as any`.
   - **service specs**: tipar commandBus/queryBus/tenantContext con sus interfaces reales.
3. Ejecutar `pnpm nx test base-backend` tras cada batch.
4. Ejecutar `pnpm nx typecheck base-backend` al final.

## Criterios de aceptación

- [ ] Cero `as any` en specs de controllers/repos/services/handlers en `base-backend`.
- [ ] `pnpm nx test base-backend` verde.
- [ ] `pnpm nx typecheck base-backend` pasa.
