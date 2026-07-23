<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F74-B1 — Cross-Domain ViewModel Standardization</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F74" src="https://img.shields.io/badge/round-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

Replicar el patrón `ClientsController.ts` (Vanilla TS puro) en el resto de dominios que tengan
`logic/` vacía o con hooks acoplados a React/Angular.

## Dominios objetivo (F74)

| Dominio | Controlador existente | Acción |
|---------|----------------------|--------|
| auth | `useAuthController` | Refactor a `AuthController.ts` + adaptador |
| users | `useUsersController` | Refactor a `UsersController.ts` + adaptador |
| billing | `useBillingController` | Crear desde cero |
| inventory | `useInventoryController` | Crear desde cero |
| projects | `useProjectsController` | Crear desde cero |
| roles | `useRolesController` | Crear desde cero |
| settings | `useSettingsController` | Crear desde cero |
| tenants | `useTenantsController` | Crear desde cero |

## Reglas

1. `logic/<Domain>Controller.ts` — clase Vanilla TS sin imports de UI.
2. `logic/use<Domain>Controller.ts` — custom hook React/Next.
3. `logic/<domain>.controller.service.ts` (Angular) — injectable Signal Store.
4. Los controllers consumen `validate*` de `@base/shared/validation`.
5. Tests unitarios con `node --test` o Jest sin montar componentes visuales.
