<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F77 — Mobile Parity, Next.js Build Fix & React Base Completion</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-76-seventy-six-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F76-14b8a6?style=flat-square" /></a>
</p>

## Estado

Planificada · Lista para ejecutar

> Apertura 2026-07-23. Eje: **Completitud de capas pendientes, paridad móvil, fix de builds rotos y cierre de deuda post-F76**.

## Progreso

- [ ] F77-A1 — React base: completar capas faltantes de `settings` (`domain`, `ui`, `shared`, `testing`)
- [ ] F77-A2 — Next.js base: completar feature code real de `auth` y `users`; resolver build roto
- [ ] F77-A3 — Next.js arquetipos: completar feature code real de `auth`, `clients` y `users`
- [ ] F77-A4 — Ionic/React Native base + arquetipos: completar feature stubs vacíos y lógica mínima
- [ ] F77-A5 — Fix `libs/base/frontend/next/native-ui/features/src/native-ui.routes.tsx` (sintaxis inválida RCS)
- [ ] F77-A6 — Actualizar `docs/plans/README.md` con F77 como ronda activa
- [ ] F77-A7 — Smoke build `nx build next-single` / `react-single` / `ionic-single` / `angular-single`

## Contexto

La Ronda F76 completó el rollout físico de 8 capas en Angular, React, Next, Ionic y React Native, además de endurecer configuraciones `test`/`lint` y limpiar God Barrels. Quedaron tareas residuales:
- React base `settings` solo expone `api` + `data-access` + `features`; faltan `domain`, `ui`, `shared`, `testing`.
- Next.js apps (`next-single`) no compilan por deuda de código en `features` y un archivo `native-ui.routes.tsx` con sintaxis RCS rota.
- Ionic y RN base/arquetipos quedaron con carpetas pobladas pero con código stub mínimo; falta paridad de feature code.
- La documentación de rondas sigue apuntando a F72 como activa.

## Objetivos Clave de la Ronda F77

1. **Completar capas faltantes**: llenar `domain`/`ui`/`shared`/`testing` donde solo existan las 4 capas core.
2. **Paridad móvil**: llevar Ionic/RN a paridad funcional mínima (pages, layout, logic) con Angular/React base.
3. **Fix builds rotos**: resolver `@base/shared` en Next features y el syntax error de `native-ui.routes.tsx`.
4. **Documentación**: actualizar `docs/plans/README.md` y cerrar F76 formalmente.

## Tareas

### F77-A1 — React base: settings 8-layer completion

- Crear/verificar `libs/base/frontend/react/domains/settings/{domain,ui,shared,testing}`
- Registrar paths en `tsconfig.base.json`
- Registrar packages en `check-all-frameworks.cjs` como framework completado
- Verificar `nx build react-single`

### F77-A2 — Next.js base: feature code + build fix

- Completar `libs/base/frontend/next/{auth,users}/features/src/{pages,layout,logic}/` con código real
- Corregir imports rotos a `@base/shared` y `@base/next-*`
- Asegurar barrel exports en `libs/base/frontend/next/{auth,users}/features/src/index.ts`
- Corregir `libs/base/frontend/next/native-ui/features/src/native-ui.routes.tsx`
- Ejecutar `nx build next-single`

### F77-A3 — Next.js arquetipos: feature code completion

- Completar `libs/arquetipos/frontend/next/{auth,clients,users}/features/src/{pages,layout,logic}/`
- Asegurar barrel exports y thin re-exports correctos hacia `@base/*`

### F77-A4 — Mobile parity (Ionic + RN)

- Completar feature code mínimo en base + arquetipos:
  - Ionic: `audit`, `auth`, `clients`, `dashboard`, `roles`, `settings`, `tasks`, `users`
  - RN: `auth`, `clients`, `dashboard`, `users`
- Asegurar que `features/src/index.ts` exporta al menos un componente/página por dominio
- Verificar `check-all-frameworks.cjs` sin entradas `MISSING`

### F77-A5 — Fix native-ui.routes.tsx

- Corregir sintaxis RCS en `libs/base/frontend/next/native-ui/features/src/native-ui.routes.tsx`

### F77-A6 — Documentación

- Actualizar `docs/plans/README.md` para marcar F76 como completada y F77 como activa
- Cerrar formalmente F76 en su README

### F77-A7 — Smoke builds

- Ejecutar `nx build {angular-single,react-single,next-single,ionic-single}`

## Entregables

- `docs/plans/rounds/plans-77-seventy-seven-round/README.md`
- Code completado en `libs/base/frontend/{react,next,mobile}` y `libs/arquetipos/frontend/{next,mobile}`
- `docs/plans/README.md` actualizado
- Verde en `check-lib-layout --strict`, `check:exports-paths --strict`, `check-all-frameworks.cjs`

## Predecesora / Siguiente

- Predecesora: [F76](../plans-76-seventy-six-round/)
- Siguiente: TBD
