# F53-A2 — Paridad wrappers + catálogo `@base/native-ui`

## Estado

listo para ejecutar

## Objetivo

Solo **3/14** CEs tienen wrappers en `@base/angular-ui` / `@base/react-ui`
(`NativeButton`, `NativeAlert`, `NativeInput`). El resto obliga a features a
usar CE crudo o primitivos framework-only (en conflicto con A1).

Completar wrappers delgados (tipado + bindings) para el catálogo Lit canónico
y alinear [ui-component-catalog.yaml](../../../frontend/ui-component-catalog.yaml).

## Inventario actual (baseline)

| CE tag | Angular wrapper | React wrapper |
|--------|-----------------|---------------|
| `base-button` | sí | sí |
| `base-input` | sí | sí |
| `base-alert` | sí | sí |
| `base-badge` … `base-toast` (resto) | no | no |

Lista completa de tags: ver `libs/base/frontend/crosscutting/native-ui/src`.

## Tareas

1. Por cada CE sin wrapper: crear `NativeXComponent` / `NativeX` siguiendo el
   patrón de los 3 existentes (CUSTOM_ELEMENTS_SCHEMA / registerNativeUi).
2. Exportar en barrels públicos de `@base/angular-ui` y `@base/react-ui`.
3. Actualizar catálogo YAML + stories mínimas (si B2 ya existe, preferir SB
   native; si no, stories del wrapper).
4. Tests smoke (render + un input/output) por wrapper o por grupo.
5. Documentar exclusiones (si algún CE es “host-only” sin wrapper tipado).

## Criterios de aceptación

- [ ] Wrappers Angular + React para todos los CEs canónicos **o** exclusiones
      listadas en Resultado con justificación.
- [ ] Catálogo YAML refleja native vs wrapper vs legacy framework.
- [ ] `pnpm nx typecheck base-angular-ui base-react-ui base-native-ui` verde.
- [ ] Apps pueden importar wrappers sin `CUSTOM_ELEMENTS_SCHEMA` ad-hoc extra
      (salvo lo que el wrapper ya encapsule).
