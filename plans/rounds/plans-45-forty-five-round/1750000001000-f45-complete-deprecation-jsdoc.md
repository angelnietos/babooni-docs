# F45-A2 — Completar JSDoc de deprecations

## Estado

completado

## Objetivo

Agregar fecha, motivo y reemplazo explícito en el JSDoc de todas las entradas
`@deprecated` inventariadas en F44-A1.

## Entradas

- `docs/plans/rounds/plans-44-forty-four-round/deprecations-inventory.md`
- `docs/guides/deprecation-policy.md`

## Tareas

1. **Completar JSDoc en código**:
   - Para cada entrada del inventario, editar el archivo fuente y agregar:
     ```ts
     /** @deprecated <motivo>. Usar `<reemplazo>`. Deprecated since <YYYY-MM-DD>. */
     ```
   - Priorizar entradas `keep` que permanecen en el código.
2. **Actualizar inventario**:
   - Marcar cada entrada con `JSDoc completo: sí`.
3. **Verificar**:
   - `pnpm check:deprecated` pasa.
   - No hay entradas en el inventario con `JSDoc con fecha explícita: 0`.

## Restricción

No modificar firmas públicas; solo agregar comentarios JSDoc.

## Criterios de aceptación

- [ ] 19/19 entradas del inventario tienen JSDoc con fecha, motivo y reemplazo.
- [ ] `pnpm check:deprecated` pasa.

## Dependencias

- F45-A1 (eliminar deprecated remanentes) — para evitar JSDoc en símbolos que se eliminan.
