# F52-C2 — Guía npm-publish + pulido biblia (índices / getting-started)

## Estado

listo para ejecutar

## Objetivo

La biblia menciona publish en planes F51/F52 pero **falta** (o queda stub) la
guía operativa. También hay huecos de descubrimiento: índice de guías,
CONTRIBUTING-DOCS ↔ hub, troubleshooting workspace-deps, learning-path →
publish cuando exista.

## Tareas

1. Completar [npm-publish-and-versioning.md](../../../guides/npm-publish-and-versioning.md)
   (si al abrir F52 solo hay stub F51: expandir con pasos reales del tooling
   aterrizado en A1/E1).
2. Enlazar desde:
   - [guides/README.md](../../../guides/README.md)
   - [deprecation-policy.md](../../../guides/deprecation-policy.md)
   - [docs/README.md](../../../README.md) (tabla «cómo hago…» si P1)
   - [CONTRIBUTING-DOCS.md](../../../CONTRIBUTING-DOCS.md) (sección enlaces)
3. Pulir getting-started / learning-path: enlace a hygiene gates y a
   «publicar lib» solo si la guía está usable.
4. Añadir fila troubleshooting en local-development o pnpm-layout:
   `check:workspace-deps:strict` falla → declarar `workspace:*` (F52-A2).
5. Revisar links rotos obvios en hub → guides (sin refactor masivo).

## Criterios de aceptación

- [ ] Guía npm-publish enlazada y con sección **Verificación**.
- [ ] Índices guides + hub actualizados.
- [ ] CONTRIBUTING-DOCS apunta a la guía / estilo sin contradicción.
- [ ] Sin marcar F51/E1 como «completado» en docs si el tooling no existe.
