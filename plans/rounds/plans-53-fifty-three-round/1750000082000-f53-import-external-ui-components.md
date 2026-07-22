<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F53-A3 — Inventario e integración UI de proyectos externos</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Existen componentes UI en **otros proyectos** (fuera o legacy) que aún no
están en el monorepo. Hay que **inventariarlos** y definir el pipeline de
integración hacia `@base/native-ui` (preferido) o wrappers de marca — **no**
como nuevas islas Angular/React duplicadas (A1).

Esta ronda puede cerrar con **inventario + piloto 1–N componentes**; un dump
masivo puede llevarse a F54.

## Tareas

1. Listar fuentes candidatas (repos, carpetas, Storybooks externos) en
   Resultado — pedir al equipo paths concretos si faltan.
2. Matriz por componente: `→ native-ui` | `→ wrapper marca` | `defer` | `reject`
   (licencia, solapamiento con CE existente, RN-only).
3. Piloto: portar **≥ 1** componente a Lit CE + wrappers (o documentar bloqueo).
4. Checklist de import: tokens, a11y, tests, story, catálogo YAML, ownership.
5. Enlazar desde ui-strategy § “Importar desde otro proyecto”.

## Criterios de aceptación

- [ ] Inventario priorizado en Resultado (aunque el port completo sea F54).
- [ ] Pipeline escrito (pasos + Definition of Done).
- [ ] Al menos 1 piloto integrado **o** bloqueo explícito con dueño.
- [ ] Ningún import crea primitivo framework-only nuevo en base.
