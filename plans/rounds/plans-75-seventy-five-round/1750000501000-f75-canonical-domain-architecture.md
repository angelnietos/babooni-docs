<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-A1 — Frontend Canonical Domain Architecture</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrado

## Objetivo

Establecer la topología física y lógica de 8 capas para cualquier dominio frontend en el monorepo, garantizando homogeneidad entre Angular, Ionic Angular, React, Next.js y React Native.

## Organización Canónica del Dominio Frontend

```text
[domain]/
├── domain/
│   └── src/
│       ├── models/         # Modelos de negocio del frontend
│       ├── value-objects/  # Objetos de valor inmutables
│       ├── enums/          # Enumeraciones de dominio
│       ├── types/          # Tipos puros de TypeScript
│       ├── validators/     # Reglas de validación pura
│       ├── services/       # Servicios de dominio agnósticos
│       ├── contracts/      # Interfases y contratos de dominio
│       └── index.ts
│
├── api/
│   └── src/
│       ├── clients/        # Cliente HTTP, Axios, Fetch, Apollo, SDKs
│       ├── endpoints/      # Operaciones y URLs del backend
│       ├── dto/            # Data Transfer Objects (recibidos/enviados)
│       ├── mappers/        # Transformaciones DTO <-> Domain Model
│       ├── serializers/    # Serialización y normalización
│       ├── contracts/      # Contratos de la API
│       ├── interceptors/   # Interceptores HTTP/SDK
│       ├── errors/         # Normalización de errores API
│       └── index.ts
│
├── data-access/
│   └── src/
│       ├── queries/        # Queries de lectura (TanStack Query / Signals)
│       ├── mutations/      # Mutaciones de escritura
│       ├── store/          # Estado local UI (Signals/SignalStore o Zustand)
│       ├── selectors/      # Selectores de estado computado
│       ├── facades/        # Fachadas de acceso simplificado
│       ├── adapters/       # Adaptadores de persistencia/cache
│       ├── cache/          # Configuración de políticas de caché
│       └── index.ts
│
├── features/
│   └── src/
│       ├── pages/          # Pantallas y vistas ruteables
│       ├── components/     # Componentes específicos de caso de uso
│       ├── forms/          # Orquestación de formularios de feature
│       ├── dialogs/        # Modales y drawers
│       ├── services/       # Controladores / ViewModels (Angular/Ionic)
│       ├── hooks/          # Hooks controladores / ViewModels (React/Next/RN)
│       ├── routes/         # Definición de rutas del feature
│       ├── guards/         # Guardias de navegación del feature
│       └── index.ts
│
├── shell/
│   └── src/
│       ├── layout/         # Shell layout (Header, Sidebar, Content wrapper)
│       ├── navigation/     # Configuración de menús y subnavegación
│       ├── sidebar/        # Componentes de menú lateral del dominio
│       ├── toolbar/        # Barras de herramientas globales del dominio
│       ├── providers/      # Providers de DI, QueryClient, Contexts
│       └── index.ts
│
├── ui/
│   └── src/
│       ├── components/     # Componentes visuales reutilizables sin estado
│       ├── directives/     # Directivas de presentación (Angular/Ionic)
│       ├── pipes/          # Formateadores / Pipes de dominio
│       ├── icons/          # Iconografía específica del dominio
│       └── index.ts
│
├── shared/
│   └── src/
│       ├── utils/          # Utilidades puras
│       ├── constants/      # Constantes de dominio
│       ├── tokens/         # Injection tokens / Context keys
│       ├── helpers/        # Helpers agnósticos
│       └── index.ts
│
└── testing/
    └── src/
        ├── mocks/          # Mocks de API y Data Access
        ├── fixtures/       # Datos sintéticos de prueba
        ├── harnesses/      # Component harnesses / testing utilities
        └── index.ts
```

## Cobertura Multi-Framework y Aislamiento Físico de Librerías

Cada una de las 8 capas de cada dominio **DEBE ser una librería Nx física totalmente independiente** (`package.json` + `project.json` + `tsconfig.lib.json`) con su correspondiente Alias en TypeScript. **Este aislamiento aplica obligatoriamente a TODOS los frameworks, no únicamente a Next.js**.

| Framework | Ruta Física Base en Monorepo | Ejemplos de Paquetes Aislados (`package.json`) |
|-----------|------------------------------|-------------------------------------------------|
| **Angular** | `libs/base/frontend/angular/domains/[domain]/` | `@base/angular-[domain]-domain`<br>`@base/angular-[domain]-api`<br>`@base/angular-[domain]-data-access`<br>`@base/angular-[domain]-features`<br>`@base/angular-[domain]-shell`<br>`@base/angular-[domain]-ui`<br>`@base/angular-[domain]-shared`<br>`@base/angular-[domain]-testing` |
| **Ionic (Angular/React)** | `libs/base/frontend/ionic/[tech]/domains/[domain]/` | `@base/ionic-[tech]-[domain]-domain`<br>`@base/ionic-[tech]-[domain]-api`<br>`@base/ionic-[tech]-[domain]-data-access`<br>`@base/ionic-[tech]-[domain]-features`<br>`@base/ionic-[tech]-[domain]-shell`<br>`@base/ionic-[tech]-[domain]-ui`<br>`@base/ionic-[tech]-[domain]-shared`<br>`@base/ionic-[tech]-[domain]-testing` |
| **React** | `libs/base/frontend/react/domains/[domain]/` | `@base/react-[domain]-domain`<br>`@base/react-[domain]-api`<br>`@base/react-[domain]-data-access`<br>`@base/react-[domain]-features`<br>`@base/react-[domain]-shell`<br>`@base/react-[domain]-ui`<br>`@base/react-[domain]-shared`<br>`@base/react-[domain]-testing` |
| **Next.js** | `libs/base/frontend/next/[domain]/` | `@base/next-[domain]-domain`<br>`@base/next-[domain]-api`<br>`@base/next-[domain]-data-access`<br>`@base/next-[domain]-features`<br>`@base/next-[domain]-shell`<br>`@base/next-[domain]-ui`<br>`@base/next-[domain]-shared`<br>`@base/next-[domain]-testing` |
| **React Native** | `libs/base/frontend/react-native/domains/[domain]/` | `@base/rn-[domain]-domain`<br>`@base/rn-[domain]-api`<br>`@base/rn-[domain]-data-access`<br>`@base/rn-[domain]-features`<br>`@base/rn-[domain]-shell`<br>`@base/rn-[domain]-ui`<br>`@base/rn-[domain]-shared`<br>`@base/rn-[domain]-testing` |

## Reglas Principales de Aislamiento
1. **Aislamiento Físico Total**: Cada capa es un paquete Nx independiente con su propio `project.json` y `package.json`. No se permiten monolitos de dominio por framework.
2. **Independencia Tecnológica**: `domain/` vive en Vanilla TypeScript sin importaciones de UI ni frameworks.
3. **Restricción de Importación Remota**: La comunicación remota/HTTP queda confinada exclusivamente en la librería `api/` de cada framework/dominio.
4. **Frontera de Estado**: El manejo de estado vive únicamente dentro de la librería `data-access/` del dominio correspondiente.
