# F63-B2 — CRUD permissions grid + density

## Estado

listo para ejecutar

## Objetivo

El grid PERMISOS de roles (5+4 cards) se ve irregular; densificar `arq-crud*`
toolbar/tabla para look 2030 coherente con clients.

## Entregables

1. Grid CSS `auto-fill` estable (mismo ancho de card).
2. Alinear toolbar Crear (inputs + CTA misma baseline).
3. Tokens spacing/radius en `_crud-page.scss` (no magic numbers sueltos).

## Criterios de aceptación

- [ ] Cards de permisos alineadas en columnas regulares.
- [ ] Toolbar clients/roles: CTA alineado verticalmente con inputs.

## Verificación

Manual `/roles` + `/clients` en 1280 y 1920.

## Depende de

F63-B1 recomendado.
