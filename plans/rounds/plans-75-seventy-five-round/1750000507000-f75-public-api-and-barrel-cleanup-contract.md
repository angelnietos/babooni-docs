<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-F1 — Public API & Barrel Cleanup Contract</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrado

## Objetivo

Prohibir los barrels gigantes ("God Barrels") y los reexports transversales entre librerías no relacionadas, garantizando que cada librería exponga **exclusivamente elementos que pertenezcan a su propia responsabilidad arquitectónica**.

---

## El Problema de los "God Barrels"

En evoluciones previas del monorepo se detectó la tendencia a reexportar transversalmente librerías enteras o elementos de capas ajenas por comodidad (`export * from '@base/angular'`, `export * from '@base/clients-data-access'`).

### Ejemplos Detectados y Prohibidos
1. **`clients-api` reexportando `data-access`**:
   ```ts
   // INCORRECTO (Inversión Arquitectónica y Violación de Capas)
   export * from '@base/clients-data-access';
   ```
2. **`api` reexportando Infraestructura de Estado (Store)**:
   ```ts
   // INCORRECTO (Mezcla API con Infraestructura Frontend)
   export * from '@base/angular-store';
   ```
3. **`api` reexportando Guards de Navegación**:
   ```ts
   // INCORRECTO (Un Guard no es API; pertenece a shared/auth/guards o features)
   export * from './lib/auth/auth.guard';
   ```
4. **`api` reexportando Componentes Visuales (UI)**:
   ```ts
   // INCORRECTO (Una librería API jamás debe exportar JSX/HTML/Componentes)
   export * from './session-expiry-host.component';
   ```
5. **Reexportar una librería entera (`@base/angular` o `@base/react`)**:
   ```ts
   // INCORRECTO (Rompe los límites del paquete y oculta las dependencias reales)
   export * from '@base/angular';
   ```

---

## Reglas de la API Pública por Librería

### Regla 1: Responsabilidad Estricta de Exportación
Una librería solo puede exportar símbolos cuya responsabilidad pertenezca explícitamente a su capa.

#### Matriz de Símbolos Permitidos y Prohibidos en `api/`
| Tipo de Símbolo | ¿Permitido en `[domain]-api`? | Ubicación Correcta |
|-----------------|------------------------------|--------------------|
| HTTP / GraphQL / SDK Clients | **SÍ** | `[domain]-api` |
| DTOs (Interfaces de Backend) | **SÍ** | `[domain]-api` o `@base/shared` |
| Endpoints y URLs de API | **SÍ** | `[domain]-api` |
| Mappers (DTO ↔ Domain Model) | **SÍ** | `[domain]-api` |
| Interceptores HTTP y Error Normalizers | **SÍ** | `[domain]-api` |
| State Queries / Mutations / Stores / Facades | **NO** | `[domain]-data-access` |
| Route Guards | **NO** | `[domain]-features` o `shared/auth/guards` |
| Componentes Visuales / Layouts | **NO** | `[domain]-ui` o `[domain]-features` |
| Directivas / Pipes | **NO** | `[domain]-ui` |

---

### Regla 2: Prohibición de Reexports Transversales
Queda estrictamente prohibido que una librería haga `export * from '@base/otra-libreria'`.

Un consumidor debe importar explícitamente cada paquete según su necesidad:
```ts
// CORRECTO: Importaciones explícitas desde los paquetes arquitectónicamente correctos
import { ClientsApiClient } from '@base/angular-clients-api';
import { useClientsQuery } from '@base/react-clients-data-access';
import { ClientBadge } from '@base/react-clients-ui';
```

---

### Regla 3: Únicas Excepciones Permitidas de Reexportación
Solo se permiten **dos clases** de reexportaciones en un archivo `index.ts`:

1. **Barrel Interno Propio**: Reexportar únicamente submódulos internos pertenecientes al mismo directorio físico de la librería.
   ```ts
   // CORRECTO: Barrel interno propio
   export * from './lib/clients/clients-api.client';
   export * from './lib/dto/client.dto';
   ```
2. **Compatibilidad Temporal de Migración**: Reexportación de transición explícitamente anotada con un tag de deprecación y fecha límite de eliminación.
   ```ts
   // TODO F75 remove after migration [2026-08-30]
   export * from '@base/clients-data-access';
   ```

---

## Configuración Completa de Pruebas y Linting por Librería

Cada librería existente y cada nueva librería creada por la arquitectura canónica de 8 capas (`domain`, `api`, `data-access`, `features`, `shell`, `ui`, `shared`, `testing`) en cualquier framework (`angular`, `react`, `next`, `ionic`, `react-native`) **DEBE contar con**:

1. **Configuración de Tests (`project.json` + Jest/Vitest)**:
   - Archivo `jest.config.ts` o `vitest.config.ts` local.
   - Target `"test"` configurado en `project.json`.
2. **Configuración de Linting (`project.json` + ESLint)**:
   - Archivo `.eslintrc.json` o `eslint.config.js` extendiendo las reglas del monorepo.
   - Target `"lint"` configurado en `project.json`.

---

## Estrategia de Testing Unificada (F75-F2 / F76-B2)

Se ha establecido una configuración de testing por framework para maximizar la compatibilidad:

| Framework | Runner | Preset | Justificación |
|-----------|--------|--------|---------------|
| **Angular** | Jest | `jest-preset-angular` | Soporte nativo de templates HTML, componentes, DI |
| **React (libs base)** | Jest | `jest.preset.js` + `ts-jest` | Compatibilidad Nx, entorno node/jsdom |
| **Next.js (libs + apps)** | Jest | `jest.preset.next.cjs` + `ts-jest` | Unifica runner, reusa `jest.shared.cjs`, `ts-jest` para TS/TSX |
| **React Native** | Jest | `jest-expo` / `metro` | Requerido por ecosistema RN |

**Regla**: Un solo runner por framework. No mezclar Jest + Vitest en el mismo scope de librería.
**Validación CI**: `F75-RULE-8` exige `test` target + config local en cada `project.json`.

---

## Completitud Estructural (F76-B2 Follow-up)

Para que `test` / `lint` targets sean válidos, la librería debe ser **estructuralmente completa**:
- `project.json` existente
- `package.json` con `name`, `main`/`exports` válidos
- `tsconfig.lib.json` (o `tsconfig.json`) para build
- `src/index.ts[x]` entry point válido (no vacío, `export {}`)
- `jest.config.ts` / `vitest.config.ts` + `tsconfig.spec.json` si aplica `test`

El caso `libs/base/frontend/react/domains/settings/features/` era **incompleto** (faltaba `project.json`, `tsconfig.lib.json`, `index.tsx`, `jest.config.ts`, `tsconfig.spec.json`). Se completó en F76-B2 follow-up.
