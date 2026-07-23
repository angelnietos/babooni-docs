<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F73-A1 — Isomorphic Shared Contracts & DTO Validation Alignment</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F73" src="https://img.shields.io/badge/round-F73-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

---

## Objetivo

Unificar y centralizar las definiciones de **DTOs y reglas de validación isomórficas (Zod)** entre el Backend (NestJS / Express) y los 5 paquetes Frontend (`@base/<fw>-<domain>-api`), eliminando la duplicación manual y garantizando la sincronía de tipos al 100%.

---

## Motivación

Actualmente, interfaces como `ClientDto` se encuentran redefinidas por separado en:
- `libs/base/frontend/angular/clients/api`
- `libs/base/frontend/react/clients/api`
- `libs/base/frontend/next/clients/api`
- `libs/arquetipos/backend/src/.../clients.dtos.ts`

Esta duplicación genera:
1. **Drift de DTOs:** Cambios en el backend no se reflejan automáticamente en los tipos de frontend.
2. **Doble mantenimiento de validación:** Reglas en `class-validator` (NestJS) desconectadas de los predicates de TypeScript o esquemas Zod en frontend.

---

## Solución Arquitectónica: `@base/shared-contracts`

1. **Creación de la capa de contratos isomórficos (`libs/base/shared/contracts/`):**
   ```text
   contracts/
   ├── src/
   │   ├── clients/
   │   │   ├── client.dto.ts        (Tipos e Interfaces TypeScript pura)
   │   │   ├── client.schema.ts     (Esquemas de validación Zod)
   │   │   └── index.ts
   │   ├── users/
   │   └── index.ts
   ```

2. **Reexportación en paquetes API Frontend:**
   Cada paquete de API por framework (ej. `@base/next-clients-api`) simplemente reexportará desde `@base/shared-contracts`, añadiendo únicamente extensiones específicas de cliente si fueran strictly necesarias.

3. **Integración Isomórfica en NestJS Backend:**
   NestJS importará los esquemas Zod desde `@base/shared-contracts` usando `nestjs-zod` o `zod-validation-error` en los Pipes de validación de los DTOs del backend.

---

## Entregables

1. Creación del paquete `@base/shared-contracts`.
2. Migración de los DTOs de `clients`, `users`, `roles` y `auth` al contrato isomórfico centralizado.
3. Integración de validación isomórfica Zod en los controladores de NestJS (`libs/arquetipos/backend`).
4. Actualización de las librerías `api` de Angular, React, Next, Ionic y RN para consumir los contratos centrales.

---

## Criterios de aceptación

- [ ] Modificar una propiedad en `client.dto.ts` actualiza los tipos instantáneamente en NestJS y en los 5 frameworks frontend.
- [ ] No existen definiciones redundantes de `ClientDto`, `UserDto` o `AuthDto` repartidas en carpetas `api/` individuales.
- [ ] Las validaciones de entrada en los endpoints de NestJS reutilizan exactamente los mismos esquemas Zod que el frontend.
- [ ] `pnpm nx run-many -t typecheck` pasa sin discrepancias de tipos DTO.
