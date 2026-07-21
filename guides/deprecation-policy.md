# Política de deprecación

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

- CI ejecuta `pnpm check:deprecated` (`tools/scripts/check-deprecated-usage.mjs`).
- PRs que reintroducen usos prohibidos sin marca `allowed` fallan el build.
