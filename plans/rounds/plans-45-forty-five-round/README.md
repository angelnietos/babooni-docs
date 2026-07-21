# Ronda F45 — Deprecation close-out + typecheck coverage + tooling

## Estado

completado

## Cierre

Ronda F45 completada. Mejoras de typecheck hygiene y DX continuan en
`plans-46-forty-six-round/`.

## Hitos

- [F45-A1 — Eliminar deprecated remanentes `remove-next-minor`](1750000000000-f45-remove-remaining-deprecations.md)
- [F45-A2 — Completar JSDoc de deprecations](1750000001000-f45-complete-deprecation-jsdoc.md)
- [F45-A3 — Expandir typecheck a todos los proyectos](1750000002000-f45-expand-typecheck-coverage.md)
- [F45-A4 — Automatizar compatibilidad Node ↔ Nx](1750000003000-f45-node-nx-compatibility-check.md)
- [F45-B1 — Mejora de harness de tests](1750000004000-f45-test-harness-improvements.md)
- [F45-C1 — Documentación y cierre de ronda](1750000005000-f45-documentation.md)

## Objetivo

1. **Cierre F44**: eliminar o migrar los símbolos `remove-next-minor` que siguen
   presentes en el código y que F44-A4 no alcanzó a remover.
2. **Deprecation metadata**: completar JSDoc con fecha y motivo en todas las
   entradas del inventario.
3. **Typecheck coverage**: garantizar que `typecheck` existe y pasa en todos los
   proyectos del workspace, no solo `base-backend`.
4. **Tooling**: implementar la verificación automática de compatibilidad Node ↔ Nx
   definida en el plan global `F00`.
5. **Test harness**: reducir fragilidad en specs removiendo dependencias a
   `AnyConstructor` y mejorando helpers compartidos.
6. **Documentación**: cerrar la documentación de deprecation/versioning y
   actualizar índices.

## Orden de ejecución

1. F45-A1 (eliminar deprecated remanentes) → depende de: nada.
2. F45-A2 (completar JSDoc) → depende de: F45-A1.
3. F45-A3 (expandir typecheck) → depende de: nada; puede ejecutarse en paralelo.
4. F45-A4 (Node ↔ Nx check) → depende de: F00 (plan global).
5. F45-B1 (test harness) → depende de: nada; puede ejecutarse en paralelo.
6. F45-C1 (documentación) → depende de: F45-A1, F45-A2, F45-A3.

## Riesgos

- **Breaking changes en símbolos `remove-next-minor`**: se mitiga siguiendo el
  ciclo definido en F44-A2 (Sunset header + 2 releases de gracia ya aplicados).
- **Typecheck frágil en proyectos nuevos**: mitigado por agregar el target de
  forma temprana en el scaffolding de nuevos proyectos.
- **Compatibilidad Node/Nx**: mitigado por el check automático de F45-A4.

## Criterios de aceptación

- [x] No quedan símbolos `remove-next-minor` en el código sin migrar.
- [x] Todo `@deprecated` en el scope tiene JSDoc con fecha, reemplazo y motivo.
- [x] `pnpm nx typecheck` pasa en todos los proyectos que tienen target `typecheck`.
- [x] `pnpm check:node-nx` existe y falla si Node está fuera de rango.
- [x] CI ejecuta `check:node-nx` y pasa.
- [x] Guías de deprecation/versioning actualizadas en `docs/guides/`.
