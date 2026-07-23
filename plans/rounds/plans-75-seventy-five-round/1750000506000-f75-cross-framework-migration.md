<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-E1 — Cross-Framework Migration Plan</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrado

## Objetivo

Ejecutar la migración gradual de las librerías existentes en Next.js, Angular, React, Ionic y React Native para alinearlas con la arquitectura canónica F75, corrigiendo acoplamientos indebidos y exportaciones incorrectas.

## Librerías Prioritarias a Refactorizar y Dividir por Capa

### 1. Angular (`libs/base/frontend/angular/domains/`)
- Descomponer cada dominio (`users`, `auth`, `roles`, `settings`, `clients`) en **8 paquetes Nx físicamente aislados**:
  - `libs/base/frontend/angular/domains/[domain]/domain` (`@base/angular-[domain]-domain`)
  - `libs/base/frontend/angular/domains/[domain]/api` (`@base/angular-[domain]-api`)
  - `libs/base/frontend/angular/domains/[domain]/data-access` (`@base/angular-[domain]-data-access`)
  - `libs/base/frontend/angular/domains/[domain]/features` (`@base/angular-[domain]-features`)
  - `libs/base/frontend/angular/domains/[domain]/shell` (`@base/angular-[domain]-shell`)
  - `libs/base/frontend/angular/domains/[domain]/ui` (`@base/angular-[domain]-ui`)
  - `libs/base/frontend/angular/domains/[domain]/shared` (`@base/angular-[domain]-shared`)
  - `libs/base/frontend/angular/domains/[domain]/testing` (`@base/angular-[domain]-testing`)
- Implementar TanStack Query for Angular en `data-access/queries` y Signal Store en `data-access/store`.

### 2. React (`libs/base/frontend/react/domains/`)
- Descomponer cada dominio en **8 paquetes Nx físicamente aislados**:
  - `@base/react-[domain]-domain`, `@base/react-[domain]-api`, `@base/react-[domain]-data-access`, `@base/react-[domain]-features`, `@base/react-[domain]-shell`, `@base/react-[domain]-ui`, `@base/react-[domain]-shared`, `@base/react-[domain]-testing`.
- Configurar TanStack Query + Zustand en `data-access/`.

### 3. Ionic (Angular & React) (`libs/base/frontend/ionic/`)
- Escindir carpetas monoblock en paquetes por capa (`@base/ionic-angular-[domain]-*` y `@base/ionic-react-[domain]-*`).

### 4. React Native (`libs/base/frontend/react-native/domains/`)
- Escindir dominios mobile en librerías independientes por capa (`@base/rn-[domain]-data-access`, `@base/rn-[domain]-api`, etc.).

### 5. Next.js (`libs/base/frontend/next/`)
- **`next/users/data-access`**:
  - Eliminar importación de `@base/next-users-api`. Sustituir por importación directa de DTOs desde `@base/shared` o `@base/next-users-domain`.
  - Mover funciones mock a `libs/base/frontend/next/users/api` o `testing/`.
  - Implementar custom hook `useUsersQuery` con TanStack Query y store de Zustand en `data-access/src/store/`.
- Completar las capas faltantes `domain/`, `ui/`, `shared/` y `testing/` como librerías Nx independientes para cada dominio Next.js.

## Fases de Ejecución de la Migración

```text
Fase 1: Corrección de importaciones circulares y acoplamientos (Next.js users/data-access).
Fase 2: Creación de librerías Nx independientes de 8 capas para dominios en Angular e Ionic Angular.
Fase 3: Creación de librerías Nx independientes de 8 capas para dominios en React e Ionic React.
Fase 4: Creación de librerías Nx independientes de 8 capas para dominios en Next.js y React Native.
Fase 5: Verificación global mediante `check-frontend-conventions.mjs` y `npx tsc --noEmit`.
```
