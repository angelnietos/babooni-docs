# Planes

Este directorio contiene los **planes de trabajo** del proyecto.

## Estructura

- **`rounds/`** — rondas cerradas/archivadas (`plans-{n}-{nombre}-round/`).
- **`global/`** — planes globales transversales (no ligados a una ronda).

Los planes activos de ronda se mantienen en sus carpetas dentro de `rounds/`
hasta que se cierran. Cuando una ronda termina, su carpeta permanece en git
para trazabilidad.

## Convención

- **Rondas:** una carpeta por ronda con ID `F{N}-*` (ej. `F41-*`, `F42-*`).
  Formato de carpeta: `plans-{n}-{nombre}-round/`.
- **Globales:** archivo único con ID `F00-*` para políticas transversales.
- Estado de cada plan: `listo para ejecutar` | `en progreso` | `completado`.

## Rondas archivadas

- `rounds/plans-47-forty-seven-round/` — ronda F47 en progreso (cierre deuda F43 + typecheck coverage + architecture polish).
- `rounds/plans-46-forty-six-round/` — ronda F46 completada (typecheck hygiene + DX improvements).
- `rounds/plans-45-forty-five-round/` — ronda F45 completada (cierre F44 + cobertura typecheck + compatibilidad Node/Nx + harness tests + documentación).
- `rounds/plans-44-forty-four-round/` — ronda F44 completada (deprecation hygiene + versionado + specs remanentes).
- `rounds/plans-43-forty-three-round/` — ronda F43 activa (consolidación facade + cleanup remanente F42).
- `rounds/plans-42-forty-two-round/` — ronda F42 iniciada; F42-B1 completado en F43-A1; B2/B3/F1/B4 pendientes para F43.
- `rounds/plans-41-forty-one-round/` — ronda F41 completada (deuda principal cerrada; detalle en su README).

## Planes globales

- `global/F00-node-nx-compatibility.md` — política de compatibilidad Node.js ↔ Nx y prevención de incompatibilidades de versiones.
