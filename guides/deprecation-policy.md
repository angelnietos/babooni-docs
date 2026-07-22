<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Política de deprecación</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Objetivo

Garantizar que toda deprecación de símbolos, APIs HTTP y paquetes sea predecible,
documentada y migrable sin sorpresas para consumidores internos y externos.

## Ámbito

- Paquetes: `@base/*`, `@arquetipos/*`, `@saas/*`, `@josanz/*`.
- APIs HTTP: rutas NestJS expuestas por apps `apps/*/backend/` y libs `libs/*/backend/`.
- Ciclo de versionado semver aplicable a paquetes y rutas.

## Reglas por scope

### Paquetes

| Tipo de cambio | Release | Acción |
|----------------|---------|--------|
| Bugfix / mejora interna sin rotura | `patch` | Permitido. |
| Feature nueva no breaking | `minor` | Permitido. |
| Deprecar símbolo (JSDoc + guía) | `minor` | Permitido. |
| Remover símbolo deprecado | `major` | Obligatorio. |

- Todo símbolo deprecado debe incluir JSDoc con:
  - Fecha de deprecación (`YYYY-MM-DD`).
  - Símbolo/API de reemplazo.
  - Motivo de la deprecación.
- Ejemplo de JSDoc completo:
  ```ts
  /** @deprecated Usar `setListViewSelection`. Método reemplazado por la nueva API de selección. Deprecated since 2026-07-10. */
  ```
- Prohibido eliminar símbolos deprecados sin haber cursado al menos un `minor` desde su deprecación.
- Se recomienda abrir un issue de seguimiento por cada símbolo deprecado.

### APIs HTTP

| Tipo de cambio | Release | Acción |
|----------------|---------|--------|
| Añadir ruta | `minor` | Permitido. |
| Modificar respuesta/cuerpo (no breaking) | `minor` | Permitido. |
| Deprecar ruta | `minor` | Permitido; incluir header `Sunset`. |
| Remover ruta deprecada | `major` | Obligatorio. |

- Rutas deprecadas deben incluir el header `Sunset: <date>` (formato HTTP-date).
- Gracia mínima de **2 releases menores** antes de eliminar la ruta.
- Versionado por ruta solo en breaking mayor:
  - Breaking menor: mantener `/v1/clients` y deprecar con `Sunset`.
  - Breaking mayor: introducir `/v2/clients` y eliminar `/v1/clients` en el mismo release.

## Proceso de aprobación

1. PR que introduce la deprecación incluye:
   - JSDoc completo en símbolos deprecados.
   - Guía de migración en el PR o issue vinculado.
   - Test que cubra el comportamiento deprecated (si aplica).
2. Review de arquitecto validate que el reemplazo está listo.
3. Merge a `main`.
4. Publicación en release `minor`.
5. Seguimiento: issue abierto con fecha de eliminación objetivo.

## Excepciones

- Dependencias externas (`node_modules`) quedan fuera de esta política.
- Código experimental bajo flag (`EXPERIMENTAL_*`) puede cambiar sin deprecación formal.

## Cumplimiento

- CI ejecuta `pnpm check:deprecated` (`tools/checks/check-deprecated-usage.mjs`).
- PRs que reintroducen usos prohibidos sin marca `allowed` fallan el build.

## Publicación npm

Versionar/publicar libs publicables:
[npm-publish-and-versioning.md](./npm-publish-and-versioning.md)
(F51-E1 / F52-A1). Esta política de deprecación aplica al semver de esos
paquetes; no sustituye el flujo de release.

## UI framework-only en `@base/*-ui` (F53 / F54)

Los primitivos Angular/React/Ionic **paralelos** a `@base/native-ui` están
**congelados** ([ADR 0010](../adr/adr-0010-native-ui-lit-sot.md),
[ui-strategy § freeze](../frontend/ui-strategy.md)):

| Fase | Acción |
|------|--------|
| F53+ | No añadir/evolucionar (salvo bug crítico) |
| F54-A3 | Marcar `@deprecated` + alternativa `Native*` (**hecho** 2026-07-22) |
| F58-B3 | **Defer F59** — siguen exportados desde `@base/angular-ui` / `@base/react-ui` (login-form, audit panels, stories, SaaS wrappers). Inventario consumers + unexport en F59. |
| F59+ | Remove / dejar de exportar en `major` tras ventana de política |

Sigue las reglas JSDoc de § Paquetes. Branding `@josanz/angular-ui` **no** se
depreca en bloque por este freeze.
