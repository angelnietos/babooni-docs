<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F78 — Feature Parity, Mobile Logic Cleanup & Architecture Compliance</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <img alt="previa" src="https://img.shields.io/badge/ronda-F77-14b8a6?style=flat-square" />
</p>

## Estado

Planificada · Lista para ejecutar

> Apertura 2026-07-23. Eje: **Cierre de brechas de arquitectura pendientes de F76/F77, paridad de capas en React y limpieza de convenciones en mobile**.

## Contexto

F76 completó el rollout físico de 8 capas (domain, api, data-access, features, shell, ui, shared, testing) en Angular, React, Next.js, Ionic y React Native. F77 abordó mobile parity, Next.js build fix y React base completion. Quedan pendientes:

- `libs/base/frontend/react/domains/settings/features/src` no tiene `layout/`, `pages/`, `components/` ni `settings.routes.{ts,tsx}`.
- React base: `audit`, `auth`, `clients`, `roles`, `users` solo exponen `api + data-access + features + shell`; faltan `domain`, `ui`, `shared`, `testing`.
- Mobile Ionic: las features usan `logic/` como carpeta de orquestación, pero el contrato F74-D1 exige `services/` para Angular/Ionic.
- Mobile React Native: las features usan `logic/`, pero el contrato exige `hooks/` para RN.
- El plan detallado para Angular (F75) contempla `services/` (Angular/Ionic) y `hooks/` (React/Next/RN), pero los scaffolds de F76 usaron `logic/` universalmente para acelerar el rollout.

## Objetivos Clave de la Ronda F78

1. **Paridad de capas React base**: alcanzar 8 capas en `audit`, `auth`, `clients`, `roles`, `settings`, `users`.
2. **Renombrar `logic/` → `services/` en Ionic**: actualizar imports, barrel exports y tests.
3. **Renombrar `logic/` → `hooks/` en React Native**: actualizar imports, barrel exports y tests.
4. **Verificación transversal**: `check-lib-layout --strict`, `check:exports-paths --strict`, `check:frontend-conventions`.
5. **Documentación**: actualizar `docs/plans/README.md`, cerrar F77, aperturar F78.

## Tareas

### F78-A1 — React base: 8-layer completion para `settings`

- **Estado:** Pendiente
- Dominio: `settings`
- Acciones:
  - Crear `libs/base/frontend/react/domains/settings/shell/` con `settings.routes.tsx`
  - Crear `libs/base/frontend/react/domains/settings/features/src/layout/`, `pages/`, `components/`
  - Registrar paquetes Nx y paths en `tsconfig.base.json`
  - Verificar `check-frontend-conventions` sin FAIL

### F78-A2 — React base: 8-layer completion para dominios restantes

- **Estado:** Pendiente
- Dominios: `audit`, `auth`, `clients`, `roles`, `users`
- Acciones:
  - Crear `domain/`, `ui/`, `shared/`, `testing/` en cada dominio
  - Registrar paquetes Nx y paths en `tsconfig.base.json`
  - Asegurar barrel exports correctos
  - Verificar `nx typecheck <package>` verde por paquete

### F78-A3 — Mobile Ionic: rename `logic/` → `services/`

- **Estado:** Pendiente
- Scopes: base + arquetipos
- Dominios: `audit`, `auth`, `clients`, `dashboard`, `roles`, `settings`, `tasks`, `users`
- Acciones:
  - Renombrar carpeta `logic/` a `services/` en `features/src/`
  - Actualizar barrel `features/src/index.ts`
  - Actualizar imports en `pages/`, `components/`, `services/` (si existen más)
  - Actualizar referencias en scripts de CI / checks si aplica
  - Verificar `check-frontend-conventions` sin FAIL

### F78-A4 — Mobile React Native: rename `logic/` → `hooks/`

- **Estado:** Pendiente
- Scopes: base + arquetipos
- Dominios: `auth`, `clients`, `dashboard`, `users`
- Acciones:
  - Renombrar carpeta `logic/` a `hooks/` en `features/src/`
  - Actualizar barrel `features/src/index.ts`
  - Actualizar imports en `pages/`, `components/`
  - Verificar `check-frontend-conventions` sin FAIL

### F78-A5 — Verificación transversal

- **Estado:** Pendiente
- Acciones:
  - `pnpm check:lib-layout:strict`
  - `pnpm check:exports-paths --strict`
  - `pnpm check:frontend-conventions`
  - `pnpm nx typecheck-all` (o `pnpm typecheck:all:base-arquetipos`)
  - Smoke build: `pnpm nx build {angular-single,react-single,next-single,ionic-single}`

### F78-A6 — Documentación y cierre de F77

- **Estado:** Pendiente
- Acciones:
  - Cerrar F77 formalmente en su README (marcar todos los items como completados o cancelados, reemplazar estado por "Cerrado")
  - Actualizar `docs/plans/README.md`: marcar F77 como completada, F78 como activa, mover F77 a tabla de rondas completadas
  - Aperturar F78 en este README (ya hecho)

## Checklist de Cierre de F78

- [ ] F78-A1 React base `settings` 8-layer completion
- [ ] F78-A2 React base 5 dominios restantes 8-layer completion
- [ ] F78-A3 Mobile Ionic `logic/` → `services/` (base + arquetipos)
- [ ] F78-A4 Mobile React Native `logic/` → `hooks/` (base + arquetipos)
- [ ] F78-A5 Verificación transversal verde
- [ ] F78-A6 Documentación actualizada y F77 cerrada formalmente
- [ ] Cambios commitidos y mergeados a `main`
- [ ] **F78 cerrada** (UI F79–F82 ya cerrada — ver design-system / ui-strategy)

## Predecesora / Siguiente

- Predecesora: F77 (cerrada — solo en git)
- UI posterior (F79–F82): cerrada — ver [design-system.md](../../../frontend/design-system.md) · [ui-strategy.md](../../../frontend/ui-strategy.md)
- Paralelas activas: [F83](../plans-83-eighty-three-round/) · [F84](../plans-84-eighty-four-round/)
