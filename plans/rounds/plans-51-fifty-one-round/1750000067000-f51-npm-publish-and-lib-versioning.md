# F51-E1 — Publicación npm, `.npmrc` y versionado de libs (`@base` / UI / dominio)

## Estado

completado

## Objetivo

Hoy las libs viven en monorepo con `"version": "0.0.0"`, `"private": true` y
`workspace:*`. Eso basta para apps internas, pero **no** hay:

1. Registro npm (privado o público) + `.npmrc` / auth CI.
2. Sistema de **versionado semver** coherente con
   [deprecation-policy.md](../../../guides/deprecation-policy.md).
3. Contrato de **dependencias publicables** (qué va en `dependencies` /
   `peerDependencies` / `devDependencies`) para que un build de app o lib
   sepa de qué paquetes depende y a qué rango de versión.
4. Pipeline de **release + publish** (Nx Release o equivalente) para scopes
   publicables: UI (`@base/angular-ui`, `@base/native-ui`, …), dominio
   (`@base/clients-*`, …), kernel (`@base/shared`, `@base/backend`, …).

## Resultado

**Oleada canario** (`tag:publishable`, `version: 0.1.0`, `private: true`):

| Package | Nx project | Notas |
|---------|------------|--------|
| `@base/shared` | `base-shared` | Sin deps workspace |
| `@base/native-ui` | `base-native-ui` | `lit` en dependencies |
| `@base/angular-ui` | `base-angular-ui` | `workspace:*` → native-ui; peers Angular/rxjs |

**Tooling:**

| Pieza | Ubicación |
|-------|-----------|
| Registry example | [`.npmrc.example`](../../../../.npmrc.example) (`.npmrc` gitignored) |
| Nx Release | `nx.json` → `release` (independent, `tag:publishable`) |
| Scripts | `pnpm check:publishable-deps`, `pnpm pack:publishable`, `pnpm release:version`, `pnpm release:publish` |
| Guía | [npm-publish-and-versioning.md](../../../guides/npm-publish-and-versioning.md) |

**Verificado localmente:**

- `pnpm check:publishable-deps` → 3 packages, 0 errors.
- `pnpm pack:publishable` → 3 tarballs semver **sin** `workspace:*` en el
  `package.json` del artefacto (`tmp/publishable-packs/`).
- `pnpm nx release version patch --dry-run --first-release` → bumps 0.1.0→0.1.1
  en los tres manifests (dry-run).

**Defer / F52-A1:** publish real a registry (`private: false` + token), oleada
amplia (`react-ui`, dominio clients), job CI manual/tag, conventional commits.

## Criterios de aceptación

- [x] Inventario publishable vs private documentado.
- [x] `.npmrc.example` + guía de auth CI/local.
- [x] `nx.json` `release` operativa en dry-run.
- [x] Al menos **3 libs** canario: `npm pack` con semver y deps sin `workspace:*`.
- [x] Regla deps/peers escrita y chequeable (`check:publishable-deps`).
- [x] Guía enlazada desde guides + deprecation.
- [x] Ningún publish accidental en PRs (`private: true`; publish solo manual/tag).
