<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F52-A2 — Cerrar `check:workspace-deps:strict` (undeclared imports)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Resultado

**Antes:** 22 violaciones. **Después:** `pnpm check:workspace-deps:strict` → 0.

Declarado `workspace:*` en consumidores (shared-store `devDependencies` `@base/react`;
ionic-auth-shell → clients/dashboard shells; react-store/audit-da → `@base/shared`;
josanz documents/settings; arquetipos backend + angular/react features; react apps;
verifactu-crm-api seed).

Nota ya en [pnpm-layout.md](../../../runbooks/pnpm-layout.md) § Workspace protocol.

## Criterios de aceptación

- [x] `pnpm check:workspace-deps:strict` exit 0.
- [x] Sin nuevas violaciones module-boundaries detectadas en este cambio.
- [x] Lockfile actualizado (`pnpm install`).
