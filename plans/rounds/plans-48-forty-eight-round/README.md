# Ronda F48 — Typecheck hygiene + barrel validation + any-cast cleanup

## Estado

completado

## Progreso

- **F48-A1** completada: `josanz-angular-ui` pasa typecheck.
- **F48-A2** completada: eliminados `as any` residuales en specs de `base-backend`.
- **F48-A3** completada: eliminados imports profundos restantes en `base-backend`.
- **F48-A4** completada: barrel exports de `@base/backend` validados.

## Hitos

- [x] F48-A1 — Corregir typecheck de josanz-angular-ui
- [x] F48-A2 — Limpiar `as any` residual en specs backend
- [x] F48-A3 — Eliminar imports profundos restantes en base-backend
- [x] F48-A4 — Validar barrel exports de @base/backend
- [x] F48-B1 — Documentación y cierre de ronda

## Objetivo

1. **Typecheck hygiene**: llevar `josanz-angular-ui` a verde corrigiendo moduleResolution y tipos rotos.
2. **Code quality**: eliminar `as any` residuales en specs de `base-backend`.
3. **Architecture polish**: cerrar la deuda de imports profundos que quedaron fuera de F47.
4. **Barrel validation**: asegurar que `@base/backend` exporta todo lo que `tsconfig.base.json`/consumidores necesitan.
5. **Documentación**: mantener índices alineados.

## Orden de ejecución

1. F48-A1 (josanz-angular-ui typecheck) → completada.
2. F48-A3 (deep relative imports restantes) → depende de: F48-A1. Lista para iniciar.
3. F48-A2 (any-cast cleanup) → depende de: F48-A1, F48-A3.
4. F48-A4 (barrel exports validation) → depende de: F48-A2.
5. F48-B1 (documentación) → depende de: F48-A1, F48-A2, F48-A3, F48-A4.

## Riesgos

- **Cambios en tsconfig**: mitigado por verificar typecheck incremental por proyecto.
- **Cambios en imports**: mitigado por `pnpm nx lint base-backend` y `pnpm nx test base-backend`.
- **Barrel exports**: mitigado por `pnpm check:exports-paths` y typecheck cruzado.

## Criterios de aceptación

- [ ] `pnpm check:deprecated` pasa.
- [ ] `pnpm check:exports-paths` pasa.
- [ ] `pnpm nx typecheck base-backend` pasa.
- [ ] `pnpm nx typecheck josanz-angular-ui` pasa.
- [ ] Cero `as any` en specs de `base-backend`.
- [ ] Cero `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`.
- [ ] Guías actualizadas en `docs/guides/`.
