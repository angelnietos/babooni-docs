# F86-D — Platform DX & quality bar

Estado: listo para ejecutar · 2026-07-24

## Objetivo

Definir la barra de calidad que protege la base cuando el monorepo es la
plataforma de muchas apps.

## Acciones

- [ ] Inventario de gates obligatorios en CI main (layout, db-portability, exports-paths, …)
- [ ] Documentar “definition of done” de cambio en `@base/*`
- [ ] Soft→strict donde haya gates suaves que ya no fallan
- [ ] DX scripts: un comando `pnpm platform:verify` (alias hygiene + db gates)

## Criterios

- [ ] Doc corto en runbooks o guides
- [ ] CI refleja la barra documentada
