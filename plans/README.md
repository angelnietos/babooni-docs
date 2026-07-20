# Planes activos

Este directorio contiene los **planes de trabajo en curso** del proyecto.
No es un archivo histórico: cuando una ronda se cierra, su plan se elimina de aquí
(queda en git para trazabilidad).

## Convención

- **1 plan por ronda** con ID `F{N}-*` (ej. `F41-*`, `F42-*`).
- Cada ronda tiene su propia carpeta: `plans-{n}-{nombre}-round/`.
- Estado de cada plan: `listo para ejecutar` | `en progreso` | `completado`.
- Dependencias entre planes se documentan en la sección de orden de ejecución.

## Ronda actual

- `plans-42-forty-two-round/` — ronda F42 activa (cierre de deuda remanente F41).
- `plans-41-forty-one-round/` — ronda F41 completada (deuda principal cerrada; detalle en su README).
