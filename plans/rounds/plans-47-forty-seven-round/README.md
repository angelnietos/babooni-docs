# Ronda F47 — Cierre deuda F43 + typecheck coverage + architecture polish

## Estado

listo para ejecutar

## Hitos

- [F47-A1 — Migrar controller specs a harness](1750000020000-f47-migrate-controller-specs-to-harness.md)
- [F47-A2 — Migrar repo specs a harness](1750000021000-f47-migrate-repo-specs-to-harness.md)
- [F47-A3 — Eliminar imports profundos en base-backend](1750000022000-f47-eliminate-deep-relative-imports.md)
- [F47-A4 — Corregir typecheck de josanz-angular-ui](1750000023000-f47-fix-josanz-angular-ui-typecheck.md)
- [F47-B1 — Limpiar casts `as any` en specs backend](1750000024000-f47-cleanup-any-casts-in-backend-specs.md)
- [F47-C1 — Documentación y cierre de ronda](1750000025000-f47-documentation.md)

## Objetivo

1. **Cierre F43**: completar la migración de specs a harness, eliminar imports profundos
   y reducir boilerplate en `base-backend`.
2. **Typecheck coverage**: llevar `josanz-angular-ui` a verde.
3. **Architecture polish**: reducir `as any` residuales en tests backend.
4. **Documentación**: mantener índices y guías alineados con el estado real.

## Orden de ejecución

1. F47-A1 (controller specs → harness) → depende de: nada.
2. F47-A2 (repo specs → harness) → depende de: nada.
3. F47-A3 (eliminar imports profundos) → depende de: F47-A1, F47-A2.
4. F47-A4 (josanz-angular-ui typecheck) → depende de: nada.
5. F47-B1 (limpiar `as any` backend) → depende de: F47-A1, F47-A2.
6. F47-C1 (documentación) → depende de: F47-A1, F47-A2, F47-A3, F47-A4, F47-B1.

## Riesgos

- **Cambios en specs**: mitigado por ejecutar `pnpm nx test base-backend` y verificar
  cobertura de tests.
- **Cambios en imports**: mitigado por `pnpm nx lint base-backend` y validar barrel exports.
- **Josanz angular-ui**: mitigado por typecheck incremental por dominio.

## Criterios de aceptación

- [ ] `pnpm check:deprecated` pasa.
- [ ] `pnpm check:exports-paths` pasa.
- [ ] `pnpm nx typecheck base-backend` pasa.
- [ ] `pnpm nx typecheck josanz-angular-ui` pasa.
- [ ] Cero `overrideGuard(...).useValue({ canActivate: () => true })` inline en specs de controllers.
- [ ] Cero `loadCtx`/`buildRepo` inline en specs de repositorios.
- [ ] Cero `../../../../` en `libs/base/backend/src/lib/domains/**/*.ts`.
- [ ] Cero `// @ts-expect-error TS2416` en `libs/base/backend/`.
- [ ] Guías actualizadas en `docs/guides/`.
