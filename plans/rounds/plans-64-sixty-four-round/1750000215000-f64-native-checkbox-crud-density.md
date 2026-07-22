<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F64-B3 — Native checkbox/radio + CRUD density</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Retomar [F63-B1](../plans-63-sixty-three-round/1750000202000-f63-native-checkbox-radio.md) +
[F63-B2](../plans-63-sixty-three-round/1750000203000-f63-crud-permissions-density.md):
roles/settings aún muestran inputs OS planos; densidades CRUD irregulares.

## Entregables

1. Átomos `base-checkbox` / `base-radio` (o composición nativa) en camino feliz
   Roles + Settings (Angular/React; Ionic si aplica).
2. Grid permisos: columnas alineadas, densidad toolbar `arq-crud*` consistente.
3. Stories mínimas checkbox/radio en native-ui (alimenta C1).

## Criterios de aceptación

- [ ] Roles create/edit sin checkbox OS visible en camino feliz web.
- [ ] Permissions matrix legible ≥1024px y usable en mobile.
- [ ] Settings Ionic RBAC usa mismos patrones visuales que web (o thin wrapper).

## Verificación

Manual `/roles` + `/settings` angular-multi / ionic-multi.

```bash
pnpm nx test base-native-ui
```

## Depende de

F64-C1 recomendado (stories).
