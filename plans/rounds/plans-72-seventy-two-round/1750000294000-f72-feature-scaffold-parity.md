<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F72-C1 — Canonical Feature Architecture (Multi-Framework Parity)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F72" src="https://img.shields.io/badge/round-F72-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

---

## Objetivo

Definir e implantar un **contrato único de arquitectura para todos los features frontend**, agnóstico al 100% del framework (Angular, React, Next, Ionic y React Native), de forma que:

* Todos los dominios compartan exactamente la misma organización mental y física.
* Los generadores creen siempre la misma estructura universal.
* Las validaciones impidan desviaciones futuras y acoplamiento excesivo.
* La migración sea automática cuando sea posible.
* La lógica de negocio y de presentación (view-models, stores) sea portable entre frameworks.

Esta ronda consolida el trabajo iniciado en F66-A1 (SoC) y establece el **Canonical Framework-Agnostic Feature Architecture** del monorepo.

---

## Motivación

Actualmente existen pequeñas diferencias estructurales entre stacks:
* Hooks mezclados con componentes en React.
* Lógica de panel distribuida diferentemente según el framework.
* Presencia de `services/` en Angular vs `hooks/` en React, lo que impide una visión verdaderamente agnóstica.
* Algunos stacks sin scaffold estándar.

Esto provoca mayor carga cognitiva, menor reutilización, documentación fragmentada y dificulta compartir lógica pura de TypeScript entre diferentes apps del monorepo.

---

## Contrato canónico

Todos los frameworks utilizan exactamente la misma estructura conceptual y física. Se eliminan las carpetas específicas de framework (`services/`, `hooks/`) en favor de una abstracción unificada (`logic/` o `store/`).

```text
features/
└── src/
    ├── layout/       (Composición visual, sin lógica de negocio)
    ├── pages/        (Targets de routing, glue code mínimo)
    ├── components/   (Componentes UI reutilizables del feature)
    ├── logic/        (Controladores, ViewModels, Hooks puros o Servicios TS)
    ├── routes.ts     (Configuración de rutas)
    └── index.ts      (API pública del feature)
```

### 1. `layout/`
**Responsabilidad:** Composición visual del dominio (chrome, wrappers, outlets, tabs, split views).
**Restricción:** Nunca contiene llamadas HTTP, estado global o reglas de negocio.

### 2. `pages/`
**Responsabilidad:** Targets de enrutamiento. Su misión es conectar el router con el layout y los componentes.
**Restricción:** Código extremadamente fino (glue code). Sin lógica condicional compleja.

### 3. `components/`
**Responsabilidad:** Componentes UI reutilizables dentro del feature (presentacionales o contenedores ligeros).
**Restricción:** No realizan data fetching directo ni coordinan flujos complejos; delegan en `logic/`.

### 4. `logic/` (Reemplaza `services/` y `hooks/`)
**Responsabilidad:** Coordinar el estado del panel, la interacción de UI y la conexión con `data-access/`.
**Contenido:**
- Clases de TypeScript agnósticas (ViewModels/Controllers).
- Máquinas de estado (XState).
- Stores locales (Zustand vanilla, Signals puros).
- *Adaptadores* mínimos para el framework (ej. un custom hook que envuelve una clase TS, o un Injectable que expone un store).
**Restricción:** Es la única capa del feature autorizada para orquestar lógica compleja. Sin acoplamiento a componentes de UI.

### 5. `routes.ts` e `index.ts`
- **routes.ts**: Configuración pura de rutas.
- **index.ts**: Único punto de exportación pública (Barrel file).

---

## Reglas arquitectónicas universales

- **R1 (Estructura Estricta):** Todo feature debe contener `layout`, `pages`, `components` y `logic`. Sin excepciones.
- **R2 (Aislamiento de Lógica):** La orquestación vive únicamente en `logic/`. Nunca en `components/` ni en `pages/`.
- **R3 (Data Access):** Las llamadas HTTP puras y DTOs son responsabilidad de `data-access/`, nunca del feature directamente. El feature (`logic/`) consume de `data-access/`.
- **R4 (Agnosticismo Máximo):** Promover escribir el código de `logic/` en TypeScript puro (Vanilla JS/TS) para permitir que el mismo archivo pueda ser importado tanto por React, Angular o React Native con solo un wrapper mínimo.
- **R5 (Convención de Nombres):** Uso estandarizado universal (ej. `UsersPage`, `UsersLayout`, `UserCard`, `UsersController`).

---

## Excepciones (Minimizadas)

Dado el objetivo de ser 100% agnóstico, las excepciones se limitan estrictamente al *glue code* necesario por cada framework en su capa de integración:
- **Angular/Ionic:** En `logic/`, las clases pueden llevar el decorador `@Injectable()` para facilitar DI, aunque se prefiere DI manual si se comparte con React.
- **React/Next/RN:** En `logic/`, se permite exportar hooks adaptadores (`useUsersController`) que instancian las clases agnósticas o conectan el estado puro al ciclo de vida de React.
- **Next:** Las `pages/` se adaptan a la convención del App Router (`page.tsx`), pero el contenido sigue las mismas reglas finas.

---

## Alcance y Entregables

### 1. Definición y Documentación
- Actualizar *Architecture Guide* y *Frontend Conventions* con el modelo **Framework-Agnostic Canonical Feature**.
- Documentar cómo escribir ViewModels/Controllers en Vanilla TS reutilizables entre frameworks.

### 2. Generadores (Scaffolding)
- Refactorizar todos los Nx Generators para emitir unívocamente la nueva estructura universal (`layout/`, `pages/`, `components/`, `logic/`).
- Eliminar templates legacy acoplados a hooks/services.

### 3. Migración Automática
- Codemods para renombrar `hooks/` y `services/` a `logic/`.
- Ajustar importaciones relativas automáticamente.

### 4. Automatización y Validaciones (`check-frontend-conventions.mjs`)
- **Existencia:** Exigir `layout`, `pages`, `components`, `logic`.
- **Dependencias:** `components` no importa `data-access`. `layout` no importa `api`.
- **Exports:** `index.ts` es el único punto de entrada válido (strict barrel).

---

## Criterios de aceptación

- [ ] Todos los generadores frontend producen exactamente el mismo árbol de directorios independientemente del framework.
- [ ] No existen carpetas `hooks/` ni `services/` en features; todo ha migrado a `logic/`.
- [ ] `check-frontend-conventions` valida la estructura y dependencias de forma estricta en el CI.
- [ ] La documentación oficial refleja este contrato como el estándar absoluto.
- [ ] Se demuestra en un feature piloto la reutilización de código de `logic/` puro entre un framework (ej. React) y otro (ej. Angular).

---

## Verificación

```bash
node tools/checks/check-frontend-conventions.mjs
pnpm lint
pnpm typecheck:all
```

---

## Riesgos y Mitigaciones

- **Movimiento de archivos:** Aplicar codemods automáticos para resolver imports rotos.
- **Falsos positivos:** Ajustar reglas de lint de forma iterativa.
- **Regresión:** Eliminar los templates legacy inmediatamente tras actualizar los generadores.

---

## Fuera de alcance

- Cambios funcionales del dominio o UI.
- Reorganización interna de `data-access/` o `api/`.
- Refactors profundos de negocio (sólo movimiento estructural a `logic/`).
