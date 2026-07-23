<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-A2 — Layer Responsibilities and Contracts</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

Definir de forma inequívoca el contrato de responsabilidades, entradas, salidas y prohibiciones para cada una de las 8 capas del dominio frontend.

## Matriz de Contrato por Capa

| Capa | Responsabilidad Principal | Puede Depender De | Prohibiciones Absolutas |
|------|---------------------------|-------------------|------------------------|
| `domain/` | Contiene modelos de dominio, objetos de valor, tipos y validaciones agnósticas. | `shared/` | Prohibido importar frameworks (Angular, React, Next, RxJS, etc.) o librerías HTTP. |
| `api/` | Realiza peticiones HTTP/GraphQL/SDK, transforma DTOs a modelos de frontend. | `domain/`, `shared/` | Prohibido gestionar estado, caché, o importar de `data-access/`, `features/`, `shell/` o `ui/`. |
| `data-access/` | Gestiona el estado de servidor (queries/mutations) y estado global UI (store). | `api/`, `domain/`, `shared/` | Prohibido contener JSX/HTML, componentes visuales, directivas o imports de `features/` o `shell/`. |
| `features/` | Orquesta los casos de uso del dominio, vistas, páginas y controladores UI. | `data-access/`, `ui/`, `domain/`, `shared/` | Prohibido realizar llamadas HTTP directas o importar de `api/` sin pasar por `data-access/`. |
| `shell/` | Provee layouts de nivel superior, ruteo maestro, barra de navegación y providers. | `features/`, `ui/`, `shared/` | Prohibido contener lógica de negocio directa, llamadas HTTP o mutaciones de estado de dominio. |
| `ui/` | Componentes de presentación reutilizables del dominio (inputs, tarjetas, badges). | `domain/`, `shared/` | Prohibido inyectar servicios con estado, clientes HTTP o importar de `data-access/` / `features/`. |
| `shared/` | Constantes, helpers y tokens puramente utilitarios. | — | Prohibido depender de capas del dominio específico. Debe ser 100% utilitario. |
| `testing/` | Proporciona stubs, mocks y data factories para pruebas unitarias e integración. | `domain/`, `api/`, `shared/` | Prohibido importar código en los bundles de producción de la aplicación. |

## Detalle Específico de Responsabilidades

### 1. `domain/`
- **Responsabilidad**: Reglas de negocio puras en TypeScript.
- **Entrada**: Datos brutos de entrada.
- **Salida**: Modelos validados, entidades, predicados de tipo.
- **Ejemplo**: `validateUserRole(role: string): boolean`, `UserModel` interface.

### 2. `api/`
- **Responsabilidad**: Comunicación remota pura.
- **Contenido Permitido**: `clients/` (Axios/Fetch), `endpoints/` (URLs), `dto/` (interfaces backend), `mappers/` (`dtoToUser(dto): User`), interceptores HTTP.
- **Garantía y Prohibiciones**:
  - `api` NUNCA almacena estado en variables o caches globales.
  - `api` NUNCA reexporta `data-access`, stores, guards de navegación o componentes visuales.
  - Prohibidos los "God Barrels" y reexports transversales (`export * from '@base/otra-lib'`).

### 3. `data-access/`
- **Responsabilidad**: La única fuente de verdad del estado de la aplicación.
- **Implementación**:
  - Encapsula `useQuery` / `useMutation` o `injectQuery` / `injectMutation`.
  - Expone fachadas o hooks declarativos (`useUsers()`, `UsersFacade`).

### 4. `features/`
- **Responsabilidad**: Presentación inteligente y orquestación de pantalla.
- **Estructura**:
  - `pages/`: Componentes conectados a las rutas.
  - `components/`: Subcomponentes visuales del flujo.
  - `services/` (Angular) / `hooks/` (React): Controllers / ViewModels que contienen estado de UI efímero y coordinan `data-access`.

### 5. `shell/`
- **Responsabilidad**: Integración del dominio en la aplicación host.
- **Provee**: Layouts principales, guardias de ruta global del dominio, y el árbol de navegación.

## Estándar de Configuración de Pruebas y Linting

Todas las librerías creadas o migradas deben contar obligatoriamente con:
- Target `"test"` en `project.json` respaldado por `jest.config.ts` o `vitest.config.ts`.
- Target `"lint"` en `project.json` respaldado por `.eslintrc.json` o `eslint.config.js`.
