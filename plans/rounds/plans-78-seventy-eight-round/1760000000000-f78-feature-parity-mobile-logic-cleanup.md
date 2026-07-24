<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="88" alt="Arquetipos" />
</p>

<h1 align="center">F78 — Feature Parity, Mobile Logic Cleanup & Architecture Compliance</h1>

<p align="center">
  <b>Status:</b> Planificada · <a href="../plans-77-seventy-seven-round/README.md"><img alt="F77" src="https://img.shields.io/badge/preceded-F77-14b8a6?style=for-the-badge" /></a>
</p>

## Contexto

F76 completó el rollout físico de 8 capas y F77 abordó mobile parity, Next.js build fix y React base completion. Quedan pendientes:
- `libs/base/frontend/react/domains/settings/features/src` no tiene `layout/`, `pages/`, `components/` ni `settings.routes.{ts,tsx}` (FAIL en `check-frontend-conventions`).
- React base: `audit`, `auth`, `clients`, `roles`, `users` solo exponen 4 capas; faltan `domain`, `ui`, `shared`, `testing`.
- Mobile Ionic: usa `logic/` como carpeta de orquestación, pero el contrato exige `services/`.
- Mobile React Native: usa `logic/`, pero el contrato exige `hooks/`.

## F78-A1 — React base: settings 8-layer completion

- **Estado:** Pendiente
- Dominio: `settings`
- Paths: `libs/base/frontend/react/domains/settings/{shell,features/src/{layout,pages,components}}`
- Acciones:
  - Crear `shell/` con `settings.routes.tsx`
  - Crear `features/src/layout/`, `pages/`, `components/`
  - Registrar paquete Nx si no existe `base-react-settings-shell`
  - Registrar path en `tsconfig.base.json`

## F78-A2 — React base: 8-layer completion para dominios restantes

- **Estado:** Pendiente
- Dominios: `audit`, `auth`, `clients`, `roles`, `users`
- Paths: `libs/base/frontend/react/domains/{domain}/{domain,ui,shared,testing}`
- Acciones:
  - Crear directorios con `project.json`, `package.json`, `tsconfig.lib.json`, `jest.config.ts`, `tsconfig.spec.json`
  - Registrar paths en `tsconfig.base.json`
  - Actualizar barrel exports del dominio

## F78-A3 — Mobile Ionic: rename `logic/` → `services/`

- **Estado:** Pendiente
- Scopes: base + arquetipos
- Dominios: `audit`, `auth`, `clients`, `dashboard`, `roles`, `settings`, `tasks`, `users`
- Acciones:
  - Renombrar `features/src/logic/` a `features/src/services/`
  - Actualizar barrel `features/src/index.ts`
  - Actualizar imports en `pages/`, `components/`
  - Verificar `check-frontend-conventions` sin FAIL

## F78-A4 — Mobile React Native: rename `logic/` → `hooks/`

- **Estado:** Pendiente
- Scopes: base + arquetipos
- Dominios: `auth`, `clients`, `dashboard`, `users`
- Acciones:
  - Renombrar `features/src/logic/` a `features/src/hooks/`
  - Actualizar barrel `features/src/index.ts`
  - Actualizar imports en `pages/`, `components/`
  - Verificar `check-frontend-conventions` sin FAIL

## F78-A5 — Verificación transversal

- **Estado:** Pendiente
- Acciones:
  - `pnpm check:lib-layout:strict`
  - `pnpm check:exports-paths --strict`
  - `pnpm check:frontend-conventions`
  - `pnpm typecheck:all:base-arquetipos`
  - Smoke build: `pnpm nx build {angular-single,react-single,next-single,ionic-single}`

## F78-A6 — Documentación y cierre de F77

- **Estado:** Pendiente
- Acciones:
  - Cerrar F77 formalmente en su README
  - Actualizar `docs/plans/README.md`: F77 completada, F78 activa
  - Mover F77 a tabla de rondas completadas

<!-- nx configuration end-->
