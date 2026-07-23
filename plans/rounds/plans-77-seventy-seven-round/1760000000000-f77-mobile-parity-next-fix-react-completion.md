<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="88" alt="Arquetipos" />
</p>

<h1 align="center">F77 — Mobile Parity, Next.js Build Fix & React Base Completion</h1>

<p align="center">
  <b>Status:</b> Lista para ejecutar · <a href="../plans-76-seventy-six-round/README.md"><img alt="F76" src="https://img.shields.io/badge/preceded-F76-14b8a6?style=for-the-badge" /></a>
</p>

## Contexto

F76 completó el rollout físico de 8 capas, pero quedaron tareas residuales:
- React base `settings` solo expone 3 capas (`api`, `data-access`, `features`); faltan `domain`, `ui`, `shared`, `testing`.
- Next.js base `auth` y `users` tienen stubs de features que no compilan (`@base/shared` roto, `native-ui.routes.tsx` con sintaxis inválida).
- Ionic y RN base/arquetipos tienen carpetas pobladas con configs `test`/`lint` pero código mínimo; falta paridad de feature code.
- `docs/plans/README.md` sigue apuntando a F72 como ronda activa.

## Formato de ejecución

Las tareas se ejecutan **dominio por dominio, framework por framework, en el orden especificado**. El estado se actualiza en este documento a medida que cada dominio se completa.

## F77-A1 — React base: settings 8-layer completion

- **Estado:** Pendiente
- Paquetes: `base-react-settings-domain`, `base-react-settings-ui`, `base-react-settings-shared`, `base-react-settings-testing`
- Paths: `@base/react-settings-{domain,ui,shared,testing}`
- Acciones:
  - Crear directorios con `project.json`, `package.json`, `tsconfig.lib.json`, `jest.config.ts`, `tsconfig.spec.json`
  - Registrar paths en `tsconfig.base.json`

## F77-A2 — Next.js base: feature code + build fix

- **Estado:** Pendiente
- Dominios: `auth`, `users`
- Acciones:
  - Completar `features/src/{pages,layout,logic}/` con código real
  - Corregir imports rotos a `@base/shared` y `@base/next-*`
  - Asegurar barrel exports en `features/src/index.ts`
  - Corregir `libs/base/frontend/next/native-ui/features/src/native-ui.routes.tsx`
  - Ejecutar `nx build next-single`

## F77-A3 — Next.js arquetipos: feature code completion

- **Estado:** Pendiente
- Dominios: `auth`, `clients`, `users`
- Acciones:
  - Completar `features/src/{pages,layout,logic}/` con código real
  - Asegurar barrel exports y thin re-exports correctos hacia `@base/*`

## F77-A4 — Mobile parity (Ionic + RN)

- **Estado:** Pendiente
- Frameworks: Ionic Angular, React Native
- Scopes: base + arquetipos
- Acciones:
  - Completar feature code mínimo:
    - Ionic: `audit`, `auth`, `clients`, `dashboard`, `roles`, `settings`, `tasks`, `users`
    - RN: `auth`, `clients`, `dashboard`, `users`
  - Asegurar que `features/src/index.ts` exporta al menos un componente/página por dominio
  - Verificar `check-all-frameworks.cjs` sin entradas `MISSING`

## F77-A5 — Fix native-ui.routes.tsx

- **Estado:** Pendiente
- Archivo: `libs/base/frontend/next/native-ui/features/src/native-ui.routes.tsx`
- Acciones:
  - Corregir sintaxis RCS rota

## F77-A6 — Documentación

- **Estado:** Pendiente
- Acciones:
  - Actualizar `docs/plans/README.md` para marcar F76 como completada y F77 como activa
  - Cerrar formalmente F76 en su README

## F77-A7 — Smoke builds

- **Estado:** Pendiente
- Acciones:
  - Ejecutar `nx build {angular-single,react-single,next-single,ionic-single}`

## Checklist de Cierre de F77

- [ ] F77-A1 React base settings 8-layer completion
- [ ] F77-A2 Next.js base feature code + build fix
- [ ] F77-A3 Next.js arquetipos feature code completion
- [ ] F77-A4 Mobile parity (Ionic + RN)
- [ ] F77-A5 Fix native-ui.routes.tsx
- [ ] F77-A6 Documentación actualizada
- [ ] F77-A7 Smoke builds verdes
- [ ] Cambios commitidos y mergeados a `main`
- [ ] **F77 cerrada — evaluar apertura de F78**

<!-- nx configuration end-->
