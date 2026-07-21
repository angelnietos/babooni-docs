# F44-A1 — Inventario de deprecations activas

## Estado

completado

## Objetivo

Recorrer el repo para inventariar todo símbolo/ruta `@deprecated`, normalizar su documentación y marcar cada entrada con `keep` | `remove-next-minor` | `remove-next-major`.

## Alcance

- `libs/base/backend/src`
- `libs/base/frontend/**`
- `libs/arquetipos/**`
- `libs/clientes/**`
- `libs/productos-saas/**`

## Entradas

- Salida de `rg "@deprecated"` sobre el scope.
- Tabla inicial ya detectada en `README.md` de F44.

## Tareas

1. Correr búsqueda completa de `@deprecated` en el scope.
2. Clasificar cada entrada:
   - `keep`: deprecated por contrato público o compatibilidad hacia atrás.
   - `remove-next-minor`: sin call sites activos, reemplazo listo.
   - `remove-next-major`: breaking pendiente, requiere coordinación.
3. Verificar que cada `@deprecated` tenga JSDoc con:
   - Fecha de deprecación.
   - Símbolo/API reemplazo.
   - Motivo.
4. Generar tabla final en `docs/plans/rounds/plans-44-forty-four-round/deprecations-inventory.md`.

## Criterios de aceptación

- [ ] Todo `@deprecated` en scope tiene entrada en `deprecations-inventory.md`.
- [ ] Cada entrada tiene fecha, motivo y estado.
- [ ] No hay `@deprecated` sin JSDoc en código nuevo.

## Dependencias

- Ninguna.
