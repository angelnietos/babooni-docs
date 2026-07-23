<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F74-E1 — Frontend Shared Layer (Cross-Framework)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F74" src="https://img.shields.io/badge/round-F74-14b8a6?style=flat-square" /></a>
</p>

## Estado

Listo para ejecutar

## Objetivo

Definir una estructura estándar para el código compartido de frontend mediante una carpeta
`shared/` en cada librería frontend cuando exista funcionalidad reutilizable.

El objetivo es:
- reducir duplicación
- centralizar utilidades comunes
- favorecer implementaciones agnósticas al framework cuando sea posible
- facilitar el mantenimiento y la evolución del monorepo
- proporcionar un contrato claro para desarrolladores y agentes de IA

## Motivación

Actualmente parte del código compartido termina distribuido entre distintos dominios o
implementado varias veces para cada framework.

Ejemplos habituales:
- guards
- servicios de autenticación
- validadores
- utilidades
- mappers
- constantes
- configuración
- interceptores
- helpers

Esto provoca:
- duplicación
- comportamientos inconsistentes
- mantenimiento más costoso
- mayor dificultad para compartir mejoras

## Principios

### P1. Compartir antes que duplicar

Antes de implementar una funcionalidad específica para un framework debe evaluarse si puede
compartirse. Siempre que sea posible se debe construir una implementación reutilizable.

### P2. Framework-agnostic first

Siempre que una funcionalidad no dependa de Angular, React, Ionic o React Native debe
implementarse de forma agnóstica.

Ejemplos:
- ✅ parsing
- ✅ validaciones
- ✅ mappers
- ✅ helpers
- ✅ reglas de negocio del frontend
- ✅ adapters

### P3. Adaptadores mínimos

Cuando un framework requiera una integración específica, ésta debe limitarse a un adaptador
muy pequeño.

Ejemplo:
```text
shared/
  auth/
    auth-core.ts      ← Toda la lógica vive aquí
    angular/          ← Adaptador mínimo
    react/            ← Adaptador mínimo
    ionic/            ← Adaptador mínimo
```

## Estructura propuesta

```text
shared/
├── auth/
├── guards/
├── services/
├── hooks/
├── providers/
├── interceptors/
├── utils/
├── validation/
├── constants/
├── config/
├── types/
├── models/
├── mappers/
├── tokens/
└── index.ts
```

No todas las carpetas son obligatorias. Solo deben existir cuando aporten valor.

## Responsabilidades

### shared/auth

Responsabilidad: Todo lo relacionado con autenticación reutilizable.

Ejemplos:
- token helpers
- refresh token
- session helpers
- permisos

### shared/guards

Responsabilidad: Guards reutilizables.

Ejemplos:
- AuthGuard
- RoleGuard
- PermissionGuard

En React podrán materializarse como componentes o hooks equivalentes.

### shared/services

Responsabilidad: Servicios compartidos entre múltiples dominios.

Ejemplos:
- DialogService
- NotificationService
- StorageService
- ClipboardService

Nunca deben contener lógica específica de un dominio.

### shared/hooks

React / Next / React Native. Hooks reutilizables por varios dominios.

Ejemplos:
- useDebounce()
- useClipboard()
- usePermission()
- useResize()
- useLocalStorage()

### shared/providers

Providers reutilizables.

Ejemplos:
- Theme Provider
- Auth Provider
- Feature Flags

### shared/interceptors

Interceptores HTTP compartidos.

Ejemplos:
- Auth
- Retry
- Logging
- Correlation ID

### shared/utils

Funciones puras reutilizables.

Ejemplos:
- formatDate()
- groupBy()
- downloadFile()
- slugify()
- uuid()
- retry()

### shared/validation

Validaciones reutilizables.

Ejemplos:
- email
- phone
- password
- nif
- cif

### shared/constants

Constantes compartidas.

### shared/config

Configuración compartida.

### shared/models

Modelos reutilizables.

### shared/types

Tipos comunes.

### shared/mappers

Transformaciones reutilizables.

### shared/tokens

Tokens de DI cuando el framework los utilice.

## Jerarquía de reutilización

Siempre se debe intentar reutilizar en este orden:

1. Shared agnóstico
2. Adaptador por framework
3. Implementación específica
4. Duplicación (último recurso)

## Criterios para decidir

| Pregunta | Respuesta | Acción |
|----------|-----------|--------|
| ¿Puede escribirse sin depender del framework? | Sí | Debe vivir en Shared |
| ¿Solo cambia la integración? | Sí | Crear un adaptador |
| ¿La lógica es idéntica? | Sí | No duplicar |
| ¿Solo cambia el ciclo de vida? | Sí | Mantener un núcleo común y adaptar únicamente el ciclo de vida |

## Ejemplos

### Malo

```text
Angular → NotificationService
React  → NotificationService
Ionic  → NotificationService
```

Tres implementaciones distintas.

### Bueno

```text
NotificationCore (agnóstico)
  ↓
Angular Adapter
React Adapter
Ionic Adapter
```

### Otro ejemplo

```text
PermissionEvaluator (único)
  ↓
Angular Guard
React Hook
React Component
RN Hook
```

Todos reutilizan el mismo evaluador.

## Reglas para agentes de IA

Un agente debe seguir el siguiente orden antes de crear código nuevo:

1. Buscar una implementación compartida.
2. Si existe, reutilizarla.
3. Si no existe, comprobar si puede hacerse agnóstica.
4. Si puede hacerse agnóstica, crearla en `shared/`.
5. Si necesita integración con un framework, crear únicamente un adaptador.
6. Solo crear implementaciones independientes cuando existan limitaciones técnicas reales.

## Criterios de aceptación

- [ ] Existe una estructura `shared/` homogénea en todas las librerías frontend donde sea necesaria.
- [ ] Las implementaciones comunes no se duplican entre frameworks.
- [ ] La lógica independiente del framework reside en un núcleo compartido.
- [ ] Cada framework implementa únicamente los adaptadores imprescindibles.
- [ ] Los agentes de IA reutilizan primero `shared/` antes de generar nuevas implementaciones.

## Principio arquitectónico

**Shared by default. Framework-specific only by necessity.**

O, en español:

> Toda funcionalidad reutilizable debe implementarse primero de forma agnóstica. Solo cuando
> exista una limitación técnica del framework se crearán adaptadores o implementaciones
> específicas.

Este principio evita que, con el tiempo, aparezcan tres o cuatro versiones distintas del mismo
servicio simplemente porque se desarrollaron para Angular, React o Ionic por separado.
