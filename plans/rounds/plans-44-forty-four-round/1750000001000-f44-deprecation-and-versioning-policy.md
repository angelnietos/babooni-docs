<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F44-A2 — Política de deprecación y versionado</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Definir reglas oficiales de deprecación y versionado para paquetes `@base/*`, `@arquetipos/*`, `@saas/*` y APIs HTTP, y dejarlas documentadas en `docs/guides/`.

## Entradas

- Resultado de F44-A1 (inventario de deprecations).
- Políticas semver existentes en `package.json` raíz.

## Tareas

1. Definir política semver por scope:
   - `patch`: bugfixes, mejoras internas.
   - `minor`: nuevas features no breaking; deprecaciones nuevas.
   - `major`: removidos de símbolos deprecados o cambios breaking de API HTTP.
2. Definir política de API HTTP:
   - Rutas deprecadas incluyen header `Sunset: <date>`.
   - Mínimo 2 releases menores de gracia antes de borrar.
   - Versionado por ruta solo en breaking mayor (`/v1/clients` → `/v2/clients`).
3. Definir política de libs:
   - Todo símbolo deprecado debe tener JSDoc con fecha y reemplazo.
   - Prohibido eliminar símbolos deprecados sin pasar por ciclo `minor → major`.
4. Escribir `docs/guides/deprecation-policy.md`.
5. Escribir `docs/guides/api-versioning.md`.

## Criterios de aceptación

- [ ] `docs/guides/deprecation-policy.md` existe y cubre símbolos y APIs HTTP.
- [ ] `docs/guides/api-versioning.md` existe y cubre rutas, headers y gracia.
- [ ] Política aprobada por el equipo.

## Dependencias

- F44-A1
