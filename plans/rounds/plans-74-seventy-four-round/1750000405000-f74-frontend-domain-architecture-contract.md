<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F74-D1 — Frontend Domain Architecture Contract</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F74" src="https://img.shields.io/badge/round-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

Definir el contrato de arquitectura frontend único para Angular, React, Next, Ionic y React Native,
basado en responsabilidades claras y separadas. Sirve como referencia para desarrolladores y agentes de IA
que creen, reorganicen o refactoricen código.

## Estructura del dominio (canónica)

```text
domain/
├── api/
├── data-access/
├── features/
│   └── src/
│       ├── layout/
│       ├── pages/
│       ├── components/
│       ├── services/      (Angular / Ionic)
│       ├── hooks/         (React / Next / React Native)
│       ├── *.routes.ts[x]
│       └── index.ts[x]
└── shell/
```

## Flujo de dependencias (obligatorio)

```text
shell → features → data-access → api
```

Dependencias siempre descendentes. No se permiten dependencias ascendentes.

## Carpeta por responsabilidad

| Carpeta | Responsabilidad | Puede depender de | Nunca contiene |
|---------|----------------|-------------------|----------------|
| `shell/` | Entrada del dominio, rutas, lazy loading, providers, guards, composición | `features/` | estado, HTTP, componentes reutilizables, lógica de presentación |
| `api/` | Comunicación con sistemas externos (REST, GraphQL, WebSocket, SSE, SDKs) | — | estado, cache, stores, hooks, services, componentes, navegación |
| `data-access/` | Estado, cache, selectors, facades, transformación de datos para UI | `api/` | componentes, layouts, páginas, routing, llamadas HTTP directas |
| `features/layout/` | Organización visual de pantallas (Header, Sidebar, Tabs, Wrappers) | `components/`, `hooks/services/`, `data-access/` | llamadas HTTP, estado del dominio, lógica de negocio |
| `features/pages/` | Destinos de rutas, composición ligera de pantallas | `layout/`, `components/`, `hooks/services/` | llamadas HTTP, estado, stores, lógica de orquestación |
| `features/components/` | Componentes reutilizables (presentacionales o smart) | `hooks/`, `services/`, `data-access/` | clientes HTTP, stores globales, navegación compleja |
| `features/services/` (Angular/Ionic) | Orquestación de pantallas, formularios, coordinación UI ↔ data-access | `data-access/` | llamadas HTTP, clientes REST |
| `features/hooks/` (React/Next/RN) | Orquestación equivalente a services para React | `data-access/` | llamadas HTTP, clientes REST |
| `*.routes.ts[x]` | Definición de rutas y lazy loading | — | lógica de negocio, estado, HTTP |
| `index.ts[x]` | API pública del feature | — | implementación interna |

## Reglas

1. Si el código realiza una petición de red, pertenece a `api/`.
2. Si el código mantiene, transforma o expone datos para la UI, pertenece a `data-access/`.
3. Si el elemento organiza la pantalla, pertenece a `features/layout/`.
4. Si el componente representa una ruta o pantalla completa, pertenece a `features/pages/`.
5. Si el elemento es reutilizable dentro del feature, pertenece a `features/components/`.
6. Si la lógica coordina una pantalla Angular/Ionic, pertenece a `features/services/`.
7. Si la lógica coordina una pantalla React/Next/RN, pertenece a `features/hooks/`.

## Refactors automáticos (agentes IA)

| Si encuentra... | Debe moverlo a... |
|-----------------|-------------------|
| `fetch`, `HttpClient`, `axios`, Apollo, GraphQL, WebSocket | `api/` |
| Signals, RxJS, NgRx, Redux, Zustand, Akita, TanStack Query, cache, facades, selectors | `data-access/` |
| Headers, Sidebars, Tabs, Wrappers, Master Detail, Layouts | `features/layout/` |
| Componentes asociados a una ruta o pantalla completa | `features/pages/` |
| Componentes reutilizables del feature | `features/components/` |
| Lógica de coordinación Angular/Ionic | `features/services/` |
| Lógica de coordinación React/Next/React Native | `features/hooks/` |
| Definición de rutas | `*.routes.ts[x]` |
| Exportaciones públicas | `index.ts[x]` |

## Gobernanza

- `check-frontend-conventions.mjs` valida la estructura física (layout/pages/components/logic).
- `check-domain-conventions.mjs` valida ausencia de DTOs locales en `api/`.
- Todo PR que mueva código entre carpetas debe respetar este contrato.
