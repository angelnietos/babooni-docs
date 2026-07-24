<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F81 — Arquetipos native-ui everywhere + wrappers-by-atom</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-80-eighty-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F80-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrada · 2026-07-24

> Apertura 2026-07-24. Eje: **features Arquetipos = mismos átomos SoT (Lit via adapters)** en Angular · React · Next · Ionic · RN, y **dejar de amontonar adapters en `wrappers/native/`** — una carpeta por componente como el resto del contrato F80.

## Contexto

F79/F80 dejaron adapters Lit y layout `atoms|chrome|composites|theme`, pero:

1. **Doble catálogo** en brand UI: `atoms/ArqButton` (CSS/premium legacy) **y** `wrappers/native/ArqNative*` (Lit). Features mezclan ambos (~20 archivos con legacy vs ~12 con `ArqNative`).
2. **`lib/wrappers/native/`** es un flat dump (~25 archivos) en `@arquetipos/arquetipos-{angular,react}-ui` (y espejo en `@base/{angular,react}-ui`) — incumple el espíritu del contrato F80 (“carpeta por componente”).
3. Next / Ionic / RN plantilla ya van a `@base/{next,ionic,react-native}-ui`, pero no hay **contrato de paridad de features** (mismo set de átomos SoT por dominio auth/clients/…).

Target layout (adapters = atoms):

```text
{ui-lib}/src/lib/
├── atoms/{atom}/
│   ├── ArqNative{Atom}.{ts,tsx}   # o Native* en @base
│   ├── *.stories.*                # opcional
│   └── index.ts
├── composites/{name}/             # confirm-dialog, table…
├── chrome/{name}/                 # solo platform
└── theme/
```

**Prohibido tras F81-B1:** nuevos archivos sueltos bajo `src/lib/wrappers/native/`. La carpeta `wrappers/` se elimina o queda vacía documentada como deprecated.

## Objetivos clave

1. Contrato: features Arquetipos **solo** consumen adapters SoT (`ArqNative*` / `@base/next-ui` / `@base/ionic-ui` / `@base/react-native-ui`).
2. Refactor `wrappers/native/*` → `atoms/{atom}/` (+ composites) en brand **y** base Angular/React.
3. Migrar features (5 runtimes) a la misma matriz de átomos por dominio.
4. Retirar o thin-alias legacy `ArqButton` / `ButtonComponent` premium CSS.
5. (Carry F80) Josanz flatten opcional / defer si sale del presupuesto.

## Planes detallados

| ID | Plan | Foco |
|----|------|------|
| F81-A1 | [Native-first contract + gate](1763000001000-f81-native-first-contract.md) | Docs + soft `check-ui-native-first` / arquetipos |
| F81-B1 | [Split wrappers/native → atoms/{name}](1763000002000-f81-split-wrappers-native-folders.md) | arquetipos + base angular/react |
| F81-C1 | [Features parity all runtimes](1763000003000-f81-features-native-parity-all-runtimes.md) | auth/clients/… × 5 stacks |
| F81-D1 | [Retire legacy Arq* CSS atoms](1763000004000-f81-legacy-arq-atom-retire.md) | YAGNI wrap vs re-export |
| F81-E1 | [Josanz folder carry (opcional)](1763000005000-f81-josanz-folder-carry.md) | Carry F80-B2 |

## Checklist de cierre F81

- [x] F81-A1 contrato + gate soft documentado
- [x] F81-B1 sin flat `wrappers/native/` en arquetipos (± base)
- [x] F81-C1 features plantilla usan SoT adapters en 5 runtimes (mín. auth + clients)
- [x] F81-D1 legacy atoms retirados o alias thin
- [x] F81-E1 hecho o explícitamente → F82
- [x] Cambios commitidos
- [x] **F81 cerrada**

## Predecesora / Siguiente

- Predecesora: [F80](../plans-80-eighty-round/)
- ADRs: [0010](../../../adr/adr-0010-native-ui-lit-sot.md) · [0011](../../../adr/adr-0011-storybook-native-ui-first.md)
- Matriz: [native-ui-adapter-matrix.md](../../../frontend/native-ui-adapter-matrix.md)
- Layout: [ui-lib-folder-layout.md](../../../frontend/ui-lib-folder-layout.md)
- Guía wrap: [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md)
- Siguiente: [**F82**](../plans-82-eighty-two-round/) — Josanz on native SoT + wrappers + folder carry
