# F51-A2 — Completar capas dashboard mobile (lib-layout)

## Estado

listo para ejecutar

## Objetivo

`check-lib-layout` avisa:

- `arquetipos/frontend/mobile/ionic/dashboard` — incomplete layers `[shell]`
- `arquetipos/frontend/mobile/rn/dashboard` — incomplete layers `[shell]`

Alinear con el patrón 4 capas (api / data-access / shell / features) o
documentar excepción permanente en biblia + script.

## Tareas

1. Comparar con un dominio mobile completo (p. ej. `clients` / `auth`).
2. Añadir capas thin faltantes (re-export `@base/*` donde aplique) **o**
   registrar excepción en [josanz-product-exceptions](../../../frontend/josanz-product-exceptions.md)
   / guía mobile + ajustar `check-lib-layout` allowlist.
3. Preferir thin stubs sobre features vacías (YAGNI).
4. `node tools/scripts/check-lib-layout.mjs` sin esos warns.

## Criterios de aceptación

- [ ] Warns de dashboard mobile resueltos o excepciones documentadas.
- [ ] Tags `runtime:*` / `layer:arquetipos` coherentes.
- [ ] Typecheck de shells afectados verde.
