# Ronda F52 — Cierre deuda F51 + gates workspace + peers + docs publish

## Estado

listo para ejecutar

> **Arranque:** tras F51-D1 (o en paralelo solo docs C2 si F51 aún cierra hitos).
> F51 sigue **activa / en progreso** — no marcar F51 `completado` desde esta carpeta.

## Dependencias externas

- F50 completada.
- F51 en curso: A1/A2 hechos; A3/B1/C*/E1/D1 pendientes o mid-impl.
- Carry explícito: **npm publish/versionado** (F51-E1), **cobertura gate CI**
  (F51-A3 → soft/strict), **workspace-deps:strict** (22 violaciones preexistentes),
  **warn lib-layout** `@josanz/storybook`.

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F52-A1 | [Carry npm publish + versionado (post F51-E1)](1750000070000-f52-carry-npm-publish-and-versioning.md) | Carry F51-E1 |
| F52-A2 | [Cerrar `check:workspace-deps:strict`](1750000071000-f52-fix-undeclared-workspace-deps.md) | F51-A1 nota / hygiene |
| F52-B1 | [Storybook: runtime tag + lib-layout](1750000072000-f52-storybook-lib-layout-runtime-tag.md) | lib-layout warn |
| F52-B2 | [Gate CI cobertura backend (soft → strict)](1750000073000-f52-ci-soft-coverage-gate.md) | Carry F51-A3 |
| F52-C1 | [Peers Angular / Ionic / RN](1750000074000-f52-peer-deps-mobile-ionic-alignment.md) | DX mobile + publish |
| F52-C2 | [Guía npm-publish + pulido biblia](1750000075000-f52-docs-npm-guide-and-bible-polish.md) | Docs / DX |
| F52-D1 | [Documentación y cierre F52](1750000076000-f52-documentation.md) | Cierre |

## Objetivo

1. Completar o endurecer lo que F51-E1 deje a medias (`.npmrc`, Nx Release,
   oleada canario, guía operativa).
2. Dejar `pnpm check:workspace-deps:strict` en verde (0 violaciones).
3. Eliminar el único warn de `check-lib-layout` en `@josanz/storybook`.
4. Pasar cobertura BE de umbral local (F51-A3) a **gate CI soft** (warn) y,
   si estable, strict.
5. Alinear `peerDependencies` mobile/Ionic/Angular para evitar dual packages.
6. Documentar publish/versionado y cerrar huecos de índice en la biblia.

## Orden de ejecución

1. **F52-A1** — solo si F51-E1 no cerró criterios; si E1 ya está `completado`,
   A1 → checklist de verificación + gaps residuales (no rehacer).
2. **F52-A2** (workspace-deps) — independiente; alto ROI para `pnpm hygiene`.
3. **F52-B1** en paralelo a A2.
4. **F52-B2** — requiere umbral/reporte de F51-A3 (o reimplementar mínimo aquí).
5. **F52-C1** — tras A1 si toca `package.json` publicables; si no, paralelo.
6. **F52-C2** — docs en cualquier momento; ideal tras A1 (guía real, no stub).
7. **F52-D1** cierre.

## Riesgos

- A1/C1: publicar con peers mal puestos o `workspace:*` sin reescritura.
- A2: declarar deps que rompen module-boundaries (`layer:*`) — validar lint.
- B2: umbral 80% flaky en CI → empezar soft (warn) antes de fail.
- Storybook: path atípico; no forzar layout de dominio de producto.

## Criterios de aceptación

- [ ] F51-E1 cerrado **o** F52-A1 con Resultado que lista gaps restantes = 0
      para oleada canario.
- [ ] `pnpm check:workspace-deps:strict` exit 0.
- [ ] `node tools/scripts/check-lib-layout.mjs` sin warn de storybook.
- [ ] Coverage: script local + job/step CI soft documentado (strict opcional).
- [ ] Guía [npm-publish-and-versioning.md](../../../guides/npm-publish-and-versioning.md)
      enlazada desde guides + hub/plans.
- [ ] Hub + plans: F52 activa o cerrada; F51 en archivo **solo** si F51-D1 lo marcó.
- [ ] `pnpm check:deprecated` + `pnpm check:exports-paths` pasan.
