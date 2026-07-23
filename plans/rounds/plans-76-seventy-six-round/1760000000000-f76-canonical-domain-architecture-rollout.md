<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="88" alt="Arquetipos" />
</p>

<h1 align="center">F76 — Canonical Domain Architecture Consolidation</h1>

<p align="center">
  <b>Status:</b> En ejecución · <a href="../plans-75-seventy-five-round/1750000501000-f75-canonical-domain-architecture.md"><img alt="F75" src="https://img.shields.io/badge/preceded-F75-14b8a6?style=for-the-badge" /></a>
</p>

## Contexto

F75 estableció la topología canónica de 8 capas por dominio, pero la implementación se limitó a **una referencia piloto** en `@base/next-clients-*`. Esta ronda completa la migración física y lógica de todos los dominios y frameworks del monorepo, garantizando aislamiento de paquete independiente (`project.json` + `package.json` + `tsconfig.lib.json`) en cada capa.

## Referencia piloto (F75-Completed)

| Capa | Paquete Nx | Path Físico |
|------|-----------|-------------|
| domain | `base-next-clients-domain` | `libs/base/frontend/next/clients/domain/` |
| api | `base-next-clients-api` | `libs/base/frontend/next/clients/api/` |
| data-access | `base-next-clients-data-access` | `libs/base/frontend/next/clients/data-access/` |
| features | `base-next-clients-features` | `libs/base/frontend/next/clients/features/` |
| shell | `base-next-clients-shell` | `libs/base/frontend/next/clients/shell/` |
| ui | `base-next-clients-ui` | `libs/base/frontend/next/clients/ui/` |
| shared | `base-next-clients-shared` | `libs/base/frontend/next/clients/shared/` |
| testing | `base-next-clients-testing` | `libs/base/frontend/next/clients/testing/` |

`tsconfig.base.json` incluye paths para `@base/next-clients-*`.

## Formato de ejecución

Las tareas se ejecutan **dominio por dominio, framework por framework, en el orden especificado**. El estado se actualiza en este documento a medida que cada dominio se completa.

## F76-A1 — Next.js base (clients)

- **Estado:** Completo
- Paquetes creados: `base-next-clients-domain`, `base-next-clients-ui`, `base-next-clients-shared`, `base-next-clients-testing`
- `tsconfig.base.json` actualizado con paths
- `base-next-clients-ui` es thin re-export de `@base/next-ui`
- Próximo domino: `next-auth`

## F76-A2 — Angular (Base + Arquetipos)

Migrar dominios Angular a la topología de 8 capas.

### F76-A2.1 — Base Angular: clients

- **Estado:** Completo
- Paquetes creados: `base-angular-clients-domain`, `base-angular-clients-ui`, `base-angular-clients-shared`, `base-angular-clients-testing`
- Paths añadidos: `@base/angular-clients-domain`, `@base/angular-clients-ui`, `@base/angular-clients-shared`, `@base/angular-clients-testing`
- `base-angular-clients-ui` es thin re-export de `@base/angular-ui`
- Próximo domino: F76-A2.2 audit

### F76-A2.2 — Base Angular: audit

- **Estado:** Pendiente
- Paquetes: `base-angular-audit-domain`, `base-angular-audit-ui`, `base-angular-audit-shared`, `base-angular-audit-testing`
- Paths: `@base/angular-audit-*`

### F76-A2.3 — Base Angular: auth

- **Estado:** Pendiente
- Paquetes: `base-angular-auth-domain`, `base-angular-auth-ui`, `base-angular-auth-shared`, `base-angular-auth-testing`
- Paths: `@base/angular-auth-*`

### F76-A2.4 — Base Angular: billing

- **Estado:** Pendiente
- Paquetes: `base-angular-billing-domain`, `base-angular-billing-ui`, `base-angular-billing-shared`, `base-angular-billing-testing`
- Paths: `@base/angular-billing-*`

### F76-A2.5 — Base Angular: inventory

- **Estado:** Pendiente
- Paquetes: `base-angular-inventory-domain`, `base-angular-inventory-ui`, `base-angular-inventory-shared`, `base-angular-inventory-testing`
- Paths: `@base/angular-inventory-*`

### F76-A2.6 — Base Angular: projects

- **Estado:** Pendiente
- Paquetes: `base-angular-projects-domain`, `base-angular-projects-ui`, `base-angular-projects-shared`, `base-angular-projects-testing`
- Paths: `@base/angular-projects-*`

### F76-A2.7 — Base Angular: roles

- **Estado:** Pendiente
- Paquetes: `base-angular-roles-domain`, `base-angular-roles-ui`, `base-angular-roles-shared`, `base-angular-roles-testing`
- Paths: `@base/angular-roles-*`

### F76-A2.8 — Base Angular: settings

- **Estado:** Pendiente
- Paquetes: `base-angular-settings-domain`, `base-angular-settings-ui`, `base-angular-settings-shared`, `base-angular-settings-testing`
- Paths: `@base/angular-settings-*`

### F76-A2.9 — Base Angular: users

- **Estado:** Pendiente
- Paquetes: `base-angular-users-domain`, `base-angular-users-ui`, `base-angular-users-shared`, `base-angular-users-testing`
- Paths: `@base/angular-users-*`

### F76-A2.10 — Arquetipos Angular: audit

- **Estado:** Pendiente
- Paquetes: `arquetipos-angular-audit-domain`, `arquetipos-angular-audit-ui`, `arquetipos-angular-audit-shared`, `arquetipos-angular-audit-testing`
- Paths: `@arquetipos/angular-audit-*`

### F76-A2.11 — Arquetipos Angular: auth

- **Estado:** Pendiente
- Paquetes: `arquetipos-angular-auth-domain`, `arquetipos-angular-auth-ui`, `arquetipos-angular-auth-shared`, `arquetipos-angular-auth-testing`
- Paths: `@arquetipos/angular-auth-*`

### F76-A2.12 — Arquetipos Angular: clients

- **Estado:** Pendiente
- Paquetes: `arquetipos-angular-clients-domain`, `arquetipos-angular-clients-ui`, `arquetipos-angular-clients-shared`, `arquetipos-angular-clients-testing`
- Paths: `@arquetipos/angular-clients-*`

### F76-A2.13 — Arquetipos Angular: roles

- **Estado:** Pendiente
- Paquetes: `arquetipos-angular-roles-domain`, `arquetipos-angular-roles-ui`, `arquetipos-angular-roles-shared`, `arquetipos-angular-roles-testing`
- Paths: `@arquetipos/angular-roles-*`

### F76-A2.14 — Arquetipos Angular: settings

- **Estado:** Pendiente
- Paquetes: `arquetipos-angular-settings-domain`, `arquetipos-angular-settings-ui`, `arquetipos-angular-settings-shared`, `arquetipos-angular-settings-testing`
- Paths: `@arquetipos/angular-settings-*`

### F76-A2.15 — Arquetipos Angular: users

- **Estado:** Pendiente
- Paquetes: `arquetipos-angular-users-domain`, `arquetipos-angular-users-ui`, `arquetipos-angular-users-shared`, `arquetipos-angular-users-testing`
- Paths: `@arquetipos/angular-users-*`

## F76-A3 — React (Base + Arquetipos)

Migrar dominios React a la topología de 8 capas.

### F76-A3.1 — Base React: audit

- **Estado:** Pendiente
- Paquetes: `base-react-audit-domain`, `base-react-audit-ui`, `base-react-audit-shared`, `base-react-audit-testing`
- Paths: `@base/react-audit-*`

### F76-A3.2 — Base React: auth

- **Estado:** Pendiente
- Paquetes: `base-react-auth-domain`, `base-react-auth-ui`, `base-react-auth-shared`, `base-react-auth-testing`
- Paths: `@base/react-auth-*`

### F76-A3.3 — Base React: clients

- **Estado:** Pendiente
- Paquetes: `base-react-clients-domain`, `base-react-clients-ui`, `base-react-clients-shared`, `base-react-clients-testing`
- Paths: `@base/react-clients-*`

### F76-A3.4 — Base React: roles

- **Estado:** Pendiente
- Paquetes: `base-react-roles-domain`, `base-react-roles-ui`, `base-react-roles-shared`, `base-react-roles-testing`
- Paths: `@base/react-roles-*`

### F76-A3.5 — Base React: settings

- **Estado:** Pendiente
- Paquetes: `base-react-settings-domain`, `base-react-settings-ui`, `base-react-settings-shared`, `base-react-settings-testing`
- Paths: `@base/react-settings-*`

### F76-A3.6 — Base React: users

- **Estado:** Pendiente
- Paquetes: `base-react-users-domain`, `base-react-users-ui`, `base-react-users-shared`, `base-react-users-testing`
- Paths: `@base/react-users-*`

### F76-A3.7 — Arquetipos React: audit

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-audit-domain`, `arquetipos-react-audit-ui`, `arquetipos-react-audit-shared`, `arquetipos-react-audit-testing`
- Paths: `@arquetipos/react-audit-*`

### F76-A3.8 — Arquetipos React: auth

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-auth-domain`, `arquetipos-react-auth-ui`, `arquetipos-react-auth-shared`, `arquetipos-react-auth-testing`
- Paths: `@arquetipos/react-auth-*`

### F76-A3.9 — Arquetipos React: clients

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-clients-domain`, `arquetipos-react-clients-ui`, `arquetipos-react-clients-shared`, `arquetipos-react-clients-testing`
- Paths: `@arquetipos/react-clients-*`

### F76-A3.10 — Arquetipos React: roles

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-roles-domain`, `arquetipos-react-roles-ui`, `arquetipos-react-roles-shared`, `arquetipos-react-roles-testing`
- Paths: `@arquetipos/react-roles-*`

### F76-A3.11 — Arquetipos React: settings

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-settings-domain`, `arquetipos-react-settings-ui`, `arquetipos-react-settings-shared`, `arquetipos-react-settings-testing`
- Paths: `@arquetipos/react-settings-*`

### F76-A3.12 — Arquetipos React: users

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-users-domain`, `arquetipos-react-users-ui`, `arquetipos-react-users-shared`, `arquetipos-react-users-testing`
- Paths: `@arquetipos/react-users-*`

## F76-A4 — Next.js (resto de dominios base + arquetipos)

Migrar el resto de dominios Next.js a la topología de 8 capas.

### F76-A4.1 — Base Next: auth

- **Estado:** Pendiente
- Paquetes: `base-next-auth-domain`, `base-next-auth-ui`, `base-next-auth-shared`, `base-next-auth-testing`
- Paths: `@base/next-auth-domain`, `@base/next-auth-ui`, `@base/next-auth-shared`, `@base/next-auth-testing`

### F76-A4.2 — Base Next: users

- **Estado:** Pendiente
- Paquetes: `base-next-users-domain`, `base-next-users-ui`, `base-next-users-shared`, `base-next-users-testing`
- Paths: `@base/next-users-*`

### F76-A4.3 — Arquetipos Next: clients

- **Estado:** Pendiente
- Paquetes: `arquetipos-next-clients-domain`, `arquetipos-next-clients-ui`, `arquetipos-next-clients-shared`, `arquetipos-next-clients-testing`
- Paths: `@arquetipos/next-clients-*`

### F76-A4.4 — Arquetipos Next: auth

- **Estado:** Pendiente
- Paquetes: `arquetipos-next-auth-domain`, `arquetipos-next-auth-ui`, `arquetipos-next-auth-shared`, `arquetipos-next-auth-testing`
- Paths: `@arquetipos/next-auth-*`

### F76-A4.5 — Arquetipos Next: users

- **Estado:** Pendiente
- Paquetes: `arquetipos-next-users-domain`, `arquetipos-next-users-ui`, `arquetipos-next-users-shared`, `arquetipos-next-users-testing`
- Paths: `@arquetipos/next-users-*`

## F76-A5 — Ionic (Angular + React)

Migrar dominios Ionic a la topología de 8 capas.

### F76-A5.1 — Ionic Angular base: audit

- **Estado:** Pendiente
- Paquetes: `base-ionic-audit-domain`, `base-ionic-audit-ui`, `base-ionic-audit-shared`, `base-ionic-audit-testing`
- Paths: `@base/ionic-audit-*`

### F76-A5.2 — Ionic Angular base: auth

- **Estado:** Pendiente
- Paquetes: `base-ionic-auth-domain`, `base-ionic-auth-ui`, `base-ionic-auth-shared`, `base-ionic-auth-testing`
- Paths: `@base/ionic-auth-*`

### F76-A5.3 — Ionic Angular base: clients

- **Estado:** Pendiente
- Paquetes: `base-ionic-clients-domain`, `base-ionic-clients-ui`, `base-ionic-clients-shared`, `base-ionic-clients-testing`
- Paths: `@base/ionic-clients-*`

### F76-A5.4 — Ionic Angular base: dashboard

- **Estado:** Pendiente
- Paquetes: `base-ionic-dashboard-domain`, `base-ionic-dashboard-ui`, `base-ionic-dashboard-shared`, `base-ionic-dashboard-testing`
- Paths: `@base/ionic-dashboard-*`

### F76-A5.5 — Ionic Angular base: roles

- **Estado:** Pendiente
- Paquetes: `base-ionic-roles-domain`, `base-ionic-roles-ui`, `base-ionic-roles-shared`, `base-ionic-roles-testing`
- Paths: `@base/ionic-roles-*`

### F76-A5.6 — Ionic Angular base: settings

- **Estado:** Pendiente
- Paquetes: `base-ionic-settings-domain`, `base-ionic-settings-ui`, `base-ionic-settings-shared`, `base-ionic-settings-testing`
- Paths: `@base/ionic-settings-*`

### F76-A5.7 — Ionic Angular base: tasks

- **Estado:** Pendiente
- Paquetes: `base-ionic-tasks-domain`, `base-ionic-tasks-ui`, `base-ionic-tasks-shared`, `base-ionic-tasks-testing`
- Paths: `@base/ionic-tasks-*`

### F76-A5.8 — Ionic Angular base: users

- **Estado:** Pendiente
- Paquetes: `base-ionic-users-domain`, `base-ionic-users-ui`, `base-ionic-users-shared`, `base-ionic-users-testing`
- Paths: `@base/ionic-users-*`

### F76-A5.9 — Ionic Angular arquetipos: audit

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-audit-domain`, `arquetipos-ionic-audit-ui`, `arquetipos-ionic-audit-shared`, `arquetipos-ionic-audit-testing`
- Paths: `@arquetipos/ionic-audit-*`

### F76-A5.10 — Ionic Angular arquetipos: auth

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-auth-domain`, `arquetipos-ionic-auth-ui`, `arquetipos-ionic-auth-shared`, `arquetipos-ionic-auth-testing`
- Paths: `@arquetipos/ionic-auth-*`

### F76-A5.11 — Ionic Angular arquetipos: clients

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-clients-domain`, `arquetipos-ionic-clients-ui`, `arquetipos-ionic-clients-shared`, `arquetipos-ionic-clients-testing`
- Paths: `@arquetipos/ionic-clients-*`

### F76-A5.12 — Ionic Angular arquetipos: dashboard

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-dashboard-domain`, `arquetipos-ionic-dashboard-ui`, `arquetipos-ionic-dashboard-shared`, `arquetipos-ionic-dashboard-testing`
- Paths: `@arquetipos/ionic-dashboard-*`

### F76-A5.13 — Ionic Angular arquetipos: roles

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-roles-domain`, `arquetipos-ionic-roles-ui`, `arquetipos-ionic-roles-shared`, `arquetipos-ionic-roles-testing`
- Paths: `@arquetipos/ionic-roles-*`

### F76-A5.14 — Ionic Angular arquetipos: settings

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-settings-domain`, `arquetipos-ionic-settings-ui`, `arquetipos-ionic-settings-shared`, `arquetipos-ionic-settings-testing`
- Paths: `@arquetipos/ionic-settings-*`

### F76-A5.15 — Ionic Angular arquetipos: tasks

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-tasks-domain`, `arquetipos-ionic-tasks-ui`, `arquetipos-ionic-tasks-shared`, `arquetipos-ionic-tasks-testing`
- Paths: `@arquetipos/ionic-tasks-*`

### F76-A5.16 — Ionic Angular arquetipos: users

- **Estado:** Pendiente
- Paquetes: `arquetipos-ionic-users-domain`, `arquetipos-ionic-users-ui`, `arquetipos-ionic-users-shared`, `arquetipos-ionic-users-testing`
- Paths: `@arquetipos/ionic-users-*`

## F76-A6 — React Native (base + arquetipos)

Migrar dominios React Native a la topología de 8 capas.

### F76-A6.1 — RN base: dashboard

- **Estado:** Pendiente
- Paquetes: `base-react-native-dashboard-domain`, `base-react-native-dashboard-ui`, `base-react-native-dashboard-shared`, `base-react-native-dashboard-testing`
- Paths: `@base/rn-dashboard-*`

### F76-A6.2 — RN base: auth

- **Estado:** Pendiente
- Paquetes: `base-react-native-auth-domain`, `base-react-native-auth-ui`, `base-react-native-auth-shared`, `base-react-native-auth-testing`
- Paths: `@base/rn-auth-*`

### F76-A6.3 — RN base: clients

- **Estado:** Pendiente
- Paquetes: `base-react-native-clients-domain`, `base-react-native-clients-ui`, `base-react-native-clients-shared`, `base-react-native-clients-testing`
- Paths: `@base/rn-clients-*`

### F76-A6.4 — RN base: users

- **Estado:** Pendiente
- Paquetes: `base-react-native-users-domain`, `base-react-native-users-ui`, `base-react-native-users-shared`, `base-react-native-users-testing`
- Paths: `@base/rn-users-*`

### F76-A6.5 — RN arquetipos: dashboard

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-native-dashboard-domain`, `arquetipos-react-native-dashboard-ui`, `arquetipos-react-native-dashboard-shared`, `arquetipos-react-native-dashboard-testing`
- Paths: `@arquetipos/react-native-dashboard-*`

### F76-A6.6 — RN arquetipos: auth

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-native-auth-domain`, `arquetipos-react-native-auth-ui`, `arquetipos-react-native-auth-shared`, `arquetipos-react-native-auth-testing`
- Paths: `@arquetipos/react-native-auth-*`

### F76-A6.7 — RN arquetipos: clients

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-native-clients-domain`, `arquetipos-react-native-clients-ui`, `arquetipos-react-native-clients-shared`, `arquetipos-react-native-clients-testing`
- Paths: `@arquetipos/react-native-clients-*`

### F76-A6.8 — RN arquetipos: users

- **Estado:** Pendiente
- Paquetes: `arquetipos-react-native-users-domain`, `arquetipos-react-native-users-ui`, `arquetipos-react-native-users-shared`, `arquetipos-react-native-users-testing`
- Paths: `@arquetipos/react-native-users-*`

## F76-A7 — Scripts y automatización

- **Estado:** Pendiente
- `tools/scripts/scaffold-domain.ts` generador de las 8 capas a partir de dominio + framework.
- Actualizar `tools/checks/check-lib-layout.mjs` para validar exactamente 8 capas por dominio físico.

## F76-B1 — Barrel & Public API Cleanup Task Force

- **Estado:** Completo
- Refactorizar `libs/base/frontend/angular/domains/clients/api/src/index.ts`: eliminar reexports de `data-access`, `guards`, componentes y stores.
- Refactorizar `libs/base/frontend/angular/platform/kernel/index.ts`: descomponer el "God Barrel" y restringir exportaciones a su dominio propio.
- Auditar todos los archivos `index.ts` en `libs/base/frontend/{angular,react,next,ionic,react-native}/...` para eliminar sentencias `export * from '@base/*'` transversales.

## F76-B2 — Hardening de Configuración de Test y Linting

- **Estado:** Completo
- Auditar y asegurar que TODA librería de dominio creada o existente cuente con:
  - `jest.config.ts` o `vitest.config.ts` local.
  - Target `"test"` configurado en `project.json`.
  - `.eslintrc.json` o `eslint.config.js` local.
  - Target `"lint"` configurado en `project.json`.

## F76-B3 — CI Gates de API Pública (`check-public-api-barrels.mjs`)

- **Estado:** Completo
- Implementar e integrar `tools/ci/check-public-api-barrels.mjs` en el pipeline de CI para bloquear en PRs cualquier nuevo "God Barrel" o reexport transversal prohibido.

## Checklist de Cierre de F76

- [x] F76-A1 Next base clients completado (predecessor F75 pilot)
- [x] F76-A2.1 Base Angular: clients (api/ui/domain/shared/testing + test/lint targets)
- [ ] F76-A2..F76-A2.10 Angular resto dominios (audit, auth, billing, inventory, projects, roles, settings, users + arquetipos)
- [ ] F76-A3 React completo (base + arquetipos)
- [ ] F76-A4 Next completo (resto base + arquetipos)
- [ ] F76-A5 Ionic completo (Angular + React, base + arquetipos)
- [ ] F76-A6 React Native completo (base + arquetipos)
- [ ] F76-A7 Scripts y automatización
- [x] F76-B1 Limpieza de "God Barrels" y reexports transversales completada (Angular api, kernel, arquetipos barrel)
- [x] F76-B2 Configuración de `test` y `lint` auditada y completa en base Angular (30 libs)
- [x] F76-B3 Script `check-public-api-barrels.mjs` verde
- [ ] `check-lib-layout --strict` en verde
- [ ] `check:exports-paths` en verde
- [ ] `nx build` humo por framework
- [ ] Cambios commitidos y mergeados a `main`
- [ ] **F76 cerrada — evaluar apertura de F77**

<!-- nx configuration end-->