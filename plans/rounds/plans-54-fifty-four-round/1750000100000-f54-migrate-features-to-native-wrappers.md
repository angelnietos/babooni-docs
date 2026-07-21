# F54-A1 — Migración masiva de features → native wrappers

## Estado

listo para ejecutar

## Objetivo

F53-C1 deja **canarios**. F54 mueve el volumen de UI de producto/plantilla a
wrappers `Native*` / `ArqNative*` / Josanz-sobre-CE, reduciendo usos de
primitivos framework-only de `@base/angular-ui` y `@base/react-ui`.

**Fuera de alcance:** reescribir todo `@josanz/angular-ui` (marca); sí alinear
**nuevas** pantallas y features arquetipos/base.

## Prerrequisitos

- F53-A2 completado (wrappers para CEs usados) o exclusiones documentadas.
- F53-A1 política publicada.

## Alcance sugerido (ajustar en Resultado)

| Prioridad | Superficie | Acción |
|-----------|------------|--------|
| P0 | `@arquetipos/angular-*-features`, `react-*-features` | Button/Input/Alert → Native* |
| P1 | Apps ionic demo + shells chrome mínimos | CE o wrapper |
| P2 | `@saas/*-features` que usen `@base/angular-ui` primitivo | Native* |
| P3 | Josanz features nuevas únicamente | checklist PR |

## Tareas

1. Inventario ripgrep: imports de `ButtonComponent` / `LibButton` / equivalentes
   desde `@base/angular-ui` y `@base/react-ui` en `features` (no en la propia UI lib).
2. Script o checklist de migración por dominio (clients, users, auth, audit…).
3. Migrar P0 completo; medir conteo before/after en Resultado.
4. Actualizar stories de features si apuntaban al primitivo viejo.
5. `pnpm nx run-many -t typecheck,lint -p <proyectos tocados>`.
6. Nota en [ui-strategy.md](../../../frontend/ui-strategy.md): “features no
   importan primitivos legacy”.

### Mapeo típico

| Legacy (ejemplo) | Destino |
|------------------|---------|
| `ButtonComponent` / React `Button` base | `NativeButton` / `ArqNativeButton` |
| Alert / banner base | `NativeAlert` |
| Input text base | `NativeInput` |
| Sin wrapper aún | Abrir gap F53-A2 / no inventar HTML |

## Criterios de aceptación

- [ ] Conteos before/after en Resultado (objetivo: −70% usos P0 o justificación).
- [ ] Typecheck/lint verde en proyectos migrados.
- [ ] Ningún primitivo framework-only **nuevo** introducido en el camino.
- [ ] Guía/PR checklist actualizada.
