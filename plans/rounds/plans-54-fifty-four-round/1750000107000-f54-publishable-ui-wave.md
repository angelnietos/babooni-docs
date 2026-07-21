# F54-C2 — Ampliar oleada npm `publishable` (UI)

## Estado

completado

## Objetivo

Canario F51/F52: `shared`, `native-ui`, `angular-ui`. Ampliar a UI relacionada
lista para registry privado:

- `@base/react-ui` (si peers OK)
- `@base/ui-tokens` (si B2 aterrizó)
- Opcional: un dominio FE canario ya previsto en E1 histórico

Sin publicar `@josanz/*` / `@saas/*` público.

## Tareas

1. Tag `publishable` + `version` + `publishConfig` + peers audit.
2. `pnpm check:publishable-deps` + `pack:publishable` (≥3 sigue; ideal ≥5).
3. Actualizar guía npm-publish matriz oleada.
4. Dry-run workflow Release publishable.
5. Decisión producto registry (GH Packages vs npm) en Resultado — no improvisar.

## Criterios de aceptación

- [ ] Nueva oleada documentada y pack dry-run verde.
- [ ] Peers coherentes con F52-C1 / F53.
- [ ] Scopes producto sin publish accidental.
