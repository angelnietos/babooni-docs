# F51-A1 — Reparar deps workspace Ionic / `pnpm install`

## Estado

listo para ejecutar

## Objetivo

Varios filtros `pnpm install` fallan con 404 npm de paquetes `@base/ionic-*`
que deberían resolverse como `workspace:*` (p. ej. `@base/ionic-dashboard-api`,
`@base/ionic-auth-data-access`). Sin esto no se puede actualizar lockfile ni
añadir paquetes nuevos de forma fiable.

## Tareas

1. Inventariar `package.json` bajo `libs/base/frontend/mobile/ionic/**` con deps
   que no existen en el workspace ni en npm.
2. Crear libs faltantes (thin stubs) **o** corregir nombres a paquetes reales.
3. `pnpm install` root verde; `pnpm check:workspace-deps:strict` sin regresiones.
4. Documentar en [local-development.md](../../../guides/local-development.md)
   si hay orden de bootstrap especial.

## Criterios de aceptación

- [ ] `pnpm install` en raíz exit 0.
- [ ] Cero referencias a `@base/ionic-*` que no sean workspace packages.
- [ ] Nota en Resultado con lista de packages creados/renombrados.
