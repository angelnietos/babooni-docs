La idea es buena, pero todavía está demasiado centrada en "crear carpetas". Yo lo convertiría en una historia de arquitectura que defina el **contrato de los features** para todos los frameworks y que además automatice su cumplimiento.

Hay varias cosas que añadiría:

* Separar claramente **definición del contrato**, **migración**, **automatización** y **validación**.
* Añadir una fase para actualizar los **generators** (si no, el problema volverá a aparecer).
* Definir excepciones explícitas para cada framework.
* Documentar qué puede vivir en cada carpeta y qué no.
* Añadir comprobaciones de dependencias (no solo existencia de carpetas).

Una versión más completa sería algo como esta.

---

# F72-C1 — Canonical Feature Architecture (Multi-Framework Parity)

## Estado

Listo para ejecutar

---

# Objetivo

Definir e implantar un **contrato único de arquitectura para todos los features frontend**, independientemente del framework (Angular, React, Next, Ionic y React Native), de forma que:

* todos los dominios compartan la misma organización mental;
* los generadores creen siempre la misma estructura;
* las validaciones impidan desviaciones futuras;
* la migración sea automática cuando sea posible.

Esta ronda consolida el trabajo iniciado en F66-A1 (SoC) y establece el **Canonical Feature Architecture** del monorepo.

---

# Motivación

Actualmente existen pequeñas diferencias entre stacks:

* hooks mezclados con componentes;
* lógica de panel distribuida;
* ausencia de `services/` o `hooks/`;
* algunos stacks sin scaffold estándar.

Esto provoca:

* mayor carga cognitiva;
* menor reutilización;
* documentación diferente según framework;
* generadores inconsistentes.

F72 elimina esas diferencias.

---

# Contrato canónico

Todos los frameworks utilizan exactamente la misma estructura conceptual.

```
features/
└── src/
    ├── layout/
    ├── pages/
    ├── components/
    ├── services/     (Angular / Ionic)
    ├── hooks/        (React / Next / RN)
    ├── *.routes.ts[x]
    └── index.ts[x]
```

## Significado de cada carpeta

### layout/

Responsabilidad:

* composición visual del dominio
* chrome
* wrappers
* outlets
* tabs
* split views

Nunca contiene:

* llamadas HTTP
* estado
* lógica de negocio

---

### pages/

Responsabilidad:

targets de routing.

Debe ser extremadamente fino.

Ejemplo:

```
route
↓

page

↓

layout

↓

components
```

---

### components/

Responsabilidad:

componentes reutilizables del feature.

Pueden ser:

* presentacionales
* smart

pero nunca deben contener:

* fetch
* navegación compleja
* coordinación de varios servicios
* lógica de panel

---

### services/

Solo Angular e Ionic.

Responsabilidad:

coordinar la lógica del panel.

Ejemplos:

```
load()

refresh()

save()

delete()

filters()

pagination()

signals()

computed()
```

Nunca:

* acceso HTTP directo (eso vive en data-access)
* reglas de negocio

---

### hooks/

Solo React, Next y React Native.

Equivalente funcional de services.

Ejemplo:

```
useUsersPage()

↓

usa

↓

queries
mutations
navigation
state
memo
callbacks
```

---

### routes

Responsabilidad:

configuración de rutas exclusivamente.

Nunca:

* lógica
* providers de negocio

---

### index

Único punto público del feature.

---

# Reglas arquitectónicas

## R1

Todos los features contienen:

```
layout
pages
components
```

sin excepción.

---

## R2

La orquestación vive únicamente en:

Angular

```
services/
```

React

```
hooks/
```

Nunca en:

```
components/
```

---

## R3

Las llamadas a API siguen siendo responsabilidad de:

```
data-access/
```

Nunca:

```
services/
hooks/
components/
```

---

## R4

Los layouts nunca conocen APIs.

---

## R5

Las pages nunca implementan lógica.

---

## R6

Los components nunca contienen coordinación de panel.

---

## R7

La estructura del dominio permanece idéntica:

```
domain/

api/

data-access/

features/

shell/
```

---

## R8

Todos los frameworks utilizan el mismo naming.

Ejemplos:

```
UsersPage

UsersLayout

UserCard

useUsers()

UsersPanelService
```

---

# Excepciones por framework

## Angular

Utiliza

```
services/
```

No utiliza

```
hooks/
```

---

## React

Utiliza

```
hooks/
```

No utiliza

```
services/
```

---

## Next

Aunque el routing sea App Router, los features mantienen:

```
pages/

components/

hooks/
```

para conservar la paridad mental.

---

## Ionic

Misma arquitectura que Angular.

---

## React Native

Aunque no exista router lazy tradicional:

```
layout/
pages/
components/
hooks/
```

se mantienen.

---

# Estado actual

(igual que ahora)

---

# Alcance

## Incluye

### 1. Definición del contrato

Documentación oficial del Canonical Feature Architecture.

---

### 2. Migración

Mover automáticamente cuando sea posible:

```
components/useUsers.ts

↓

hooks/useUsers.ts
```

Servicios inline

↓

```
services/
```

---

### 3. Generators

Actualizar todos los generators para que creen automáticamente:

```
layout/

pages/

components/

services | hooks

routes

index
```

No debe existir ningún generador antiguo.

---

### 4. Validaciones

Actualizar

```
check-frontend-conventions.mjs
```

para comprobar:

## existencia

```
layout

pages

components
```

---

## ubicación

Hooks únicamente en

```
hooks/
```

Servicios únicamente en

```
services/
```

---

## dependencias

Ejemplos:

```
components

×

data-access
```

```
layout

×

api
```

```
pages

×

http
```

---

## exports

Verificar que

```
index.ts
```

expone únicamente la API pública.

---

### 5. Documentación

Actualizar:

* Architecture Guide
* Frontend Conventions
* Feature Template

---

# Entregables

* Contrato de arquitectura publicado.
* Generators actualizados.
* Validaciones ampliadas.
* Migración de features existentes.
* Documentación actualizada.
* Excepciones Next/RN documentadas.

---

# Criterios de aceptación

* Todos los nuevos features generados cumplen el contrato automáticamente.
* Ningún feature incumple la estructura.
* No existen hooks fuera de `hooks/`.
* No existen servicios de panel fuera de `services/`.
* `check-frontend-conventions` valida estructura y dependencias.
* La documentación refleja el contrato oficial.
* Angular, React, Ionic, Next y React Native presentan la misma organización conceptual.

---

# Verificación

```bash
node tools/checks/check-frontend-conventions.mjs

pnpm lint

pnpm typecheck:all

pnpm check:lib-layout
```

---

# Riesgos

* Movimiento masivo de archivos puede afectar imports.
* Posibles falsos positivos iniciales en las validaciones.
* Generadores antiguos pueden seguir creando estructuras obsoletas si no se eliminan.

Mitigación:

* aplicar codemods automáticos;
* añadir reglas de lint;
* eliminar los templates legacy una vez completada la migración.

---

# Fuera de alcance

* Cambios funcionales del dominio.
* Reorganización de `data-access`.
* Cambios en `api`.
* Refactors de negocio.
* Modificaciones del sistema de routing propias de cada framework.
