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

## Validación Automática en CI (`check-public-api-barrels.mjs`)

Se añade el script de CI `tools/ci/check-public-api-barrels.mjs` que escanea todos los archivos `index.ts` del monorepo y **falla el build** si detecta:
- Sentencias `export * from '@base/*'` hacia otras librerías sin el tag `// TODO F75 remove after migration`.
- Exportaciones de Guards, Stores, Componentes o Directivas desde librerías clasificadas como `type:api`.
