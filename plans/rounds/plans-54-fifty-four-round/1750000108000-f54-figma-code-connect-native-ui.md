# F54-D1 — Figma / Code Connect ↔ `@base/native-ui`

## Estado

listo para ejecutar

## Objetivo

Conectar el catálogo Lit al design system en Figma (Code Connect o mapeo
documentado) para que design→code no regenere Angular/React a mano.

## Tareas

1. Inventariar componentes Figma vs CE tags.
2. Si hay MCP/Figma token: crear mappings Code Connect para button/input/alert
   como piloto.
3. Si no hay acceso: plantilla de mapeo en docs + defer técnico.
4. Enlazar desde design-system § Figma handoff.
5. No generar wrappers framework desde Figma saltándose Lit.

## Criterios de aceptación

- [ ] Piloto Code Connect **o** matriz Figma↔CE publicada sin herramienta.
- [ ] Regla “Figma → native-ui primero” en docs.
- [ ] Sin regresiones de ownership UI.
