# F50-A1 — Cobertura typecheck en apps y libs frontend

## Estado

listo para ejecutar

## Origen

Carry de [F49-A3](../plans-49-forty-nine-round/1750000042000-f49-frontend-typecheck-coverage.md).

## Objetivo

Llevar a verde `pnpm nx typecheck` en libs base FE y apps principales
(Angular, React, Next, mobile donde aplique, Josanz, SaaS canarios).

## Proyectos objetivo

### Libs base
- `@base/angular`, `@base/angular-api`, `@base/angular-ui`, `@base/angular-store`
- `@base/react`, `@base/react-api`, `@base/react-ui`, `@base/react-store`
- `@base/native-ui`, `@base/shared-store`
- Next / mobile libs presentes en el graph

### Apps
- Plantillas arquetipos Angular / React / Next
- `josanz` SPA
- `verifactu-platform`, `document-generator`
- Mobile: `react-native-single`, `ionic-single` (typecheck target si existe)

## Tareas

1. Inventario: `pnpm nx show projects --withTarget typecheck` filtrado FE.
2. Batch 1: libs `@base/*` FE.
3. Batch 2: apps arquetipos + josanz.
4. Batch 3: SaaS + mobile.
5. Tras cada batch: `pnpm check:exports-paths`.
6. Registrar fallos preexistentes vs introducidos (no esconder con `skip`).

## Criterios de aceptación

- [ ] Typecheck verde en la lista objetivo (o exclusiones documentadas en el Resultado).
- [ ] `pnpm check:exports-paths` pasa.
- [ ] Cero `TS2307` por barrels rotos en esos proyectos.
