<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F44 — Deprecation hygiene + versionado + specs remanentes</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

`completado`

> **Nota:** Verificación real en `main`:
> - `pnpm check:deprecated` pasa en CI.
> - `pnpm nx typecheck base-backend` pasa.
> - `docs/guides/deprecation-policy.md` y `docs/guides/api-versioning.md` existen.
>
> Cierre completado en F45:
> - 7 símbolos `remove-next-minor` eliminados o migrados.
> - 19/19 entradas del inventario tienen JSDoc con fecha, reemplazo y motivo.

## Hitos

- [F44-A1 — Inventario de deprecations activas](1750000000000-f44-inventory-active-deprecations.md)
- [F44-A2 — Política de deprecación y versionado](1750000001000-f44-deprecation-and-versioning-policy.md)
- [F44-A3 — Tooling / CI](1750000002000-f44-deprecation-tooling-and-ci.md)
- [F44-A4 — Eliminar / migrar deprecated activos](1750000003000-f44-remove-or-migrate-active-deprecations.md)
- [F44-B1 — Corregir specs remanentes](1750000004000-f44-fix-remaining-specs.md)
- [F44-C1 — Documentación](1750000005000-f44-documentation.md)

## Objetivo

1. **Deprecation hygiene**: inventariar deprecations activas, etiquetarlas con fecha/razón y eliminar o migrar call sites que todavía usan símbolos deprecados.
2. **Versionado**: definir política de versionado para `@base/*`, `@arquetipos/*` y APIs HTTP; acordar contract de breaking/non-breaking.
3. **CI/tooling**: agregar detección automática de uso de APIs deprecadas en build y tests.
4. **Specs remanentes**: corregir errores preexistentes del test harness que `base-backend:typecheck` reporta en `*.spec.ts`.

## Orden de ejecución

1. F44-A1 (inventario) → depende de: nada.
2. F44-A2 (política) → depende de: F44-A1.
3. F44-A3 (tooling) → depende de: F44-A1.
4. F44-A4 (eliminar deprecated) → depende de: F44-A2, F44-A3.
5. F44-B1 (specs) → depende de: nada; puede ejecutarse en paralelo.
6. F44-C1 (docs) → depende de: F44-A2, F44-A4.

## Riesgos

- **Breaking changes no coordinados**: mitigado por exigir Sunset header + 2 releases de gracia.
- **Specs frágiles**: el test harness actual usa `AnyConstructor` muy laxo; se tolera `any` en tests pero no en `lib/`.

## Criterios de aceptación

- [ ] `pnpm nx typecheck base-backend` sin errores en `lib/**/*.ts` y `*.spec.ts`.
- [ ] Todo `@deprecated` en `libs/base/backend/src` tiene entrada en el inventario con estado.
- [ ] CI ejecuta `pnpm check:deprecated` y pasa.
- [ ] docs de deprecation y versionado publicados en `docs/guides/`.

