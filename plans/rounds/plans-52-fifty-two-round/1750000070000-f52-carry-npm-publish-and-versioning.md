# F52-A1 — Carry npm publish + versionado (post F51-E1)

## Estado

listo para ejecutar

## Objetivo

F51-E1 define el sistema de publicación (`.npmrc`, Nx Release, tags
`publishable`, oleada canario `@base/*`). Si al cerrar F51 quedan gaps
(herramienta a medias, sin dry-run, sin guía), **esta ronda los cierra**
sin reescribir el diseño.

Si F51-E1 ya está `completado` con todos los criterios, este plan se reduce a
**verificación** + Resultado «nada que hacer» y se marca completado.

## Contexto (baseline al abrir F52)

| Pieza | Esperado tras F51-E1 | Si falta → trabajo aquí |
|-------|----------------------|-------------------------|
| `.npmrc` / `.npmrc.example` | Presente | Crear + documentar secretos CI |
| `nx.json` → `release` | Config independent | Añadir groups `@base` canario |
| Guía operativa | `docs/guides/npm-publish-and-versioning.md` | Completar (ver F52-C2) |
| Dry-run pack/publish | Oleada canario verde | Ejecutar y fijar `files`/`publishConfig` |
| Tag `publishable` | En libs candidatas | Inventario + tags |

## Tareas

1. Auditar estado real vs criterios de [F51-E1](../plans-51-fifty-one-round/1750000067000-f51-npm-publish-and-lib-versioning.md).
2. Completar gaps: registry example, `NPM_TOKEN` en docs CI (sin secretos),
   `nx release --dry-run` / `npm pack` en canarios.
3. Verificar que artefactos publicados **no** llevan `workspace:*` crudo.
4. Gate opcional `check:publishable-deps` (o documentar por qué se difiere).
5. Enlazar Resultado a F52-C2 (guía) y deprecation-policy.

## Criterios de aceptación

- [ ] Checklist F51-E1 100% verde **o** Residual explícito = 0 para canarios.
- [ ] `.npmrc.example` en repo (sin tokens).
- [ ] Al menos un `nx release` / pack dry-run documentado en Resultado.
- [ ] No publicar scopes producto (`@josanz`, `@saas`) sin decisión explícita.
