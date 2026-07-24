<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Ronda F79 — Lit SoT Adapter Migration & Cross-Framework Visual Parity</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans-78-seventy-eight-round/README.md"><img alt="previa" src="https://img.shields.io/badge/ronda-F78-14b8a6?style=flat-square" /></a>
  <a href="../plans-80-eighty-round/README.md"><img alt="siguiente" src="https://img.shields.io/badge/ronda-F80-0d9488?style=flat-square" /></a>
</p>

## Estado

**Cerrada** · 2026-07-24

> A1–B3 ejecutados (matriz + Next/Ionic Lit adapters + RN parity). Layout seed `atoms|chrome|composites|theme` en next/ionic/rn. C1/D1/E1 → [F80-D1](../plans-80-eighty-round/1762000005000-f80-f79-carries-sb-ci.md). Pulido carpetas + purge → [F80](../plans-80-eighty-round/).

## Contexto

ADR [0010](../../../adr/adr-0010-native-ui-lit-sot.md) fija `@base/native-ui` (Lit CE) como SoT visual en hosts DOM. ADR [0011](../../../adr/adr-0011-storybook-native-ui-first.md) exige Storybook por adapter con **paridad de catálogo**.

## Objetivos clave (resultado)

1. **Matriz canónica** — [native-ui-adapter-matrix.md](../../../frontend/native-ui-adapter-matrix.md)
2. **Next** — `Arq*` → `Native*` CE islands (`'use client'`)
3. **Ionic** — átomos SoT → Lit; chrome `ion-*` documentado
4. **RN** — tokens/API alineados; sin Lit en bundle
5. Storybook / Arquetipos / CI — **diferidos a F80-D1**

## Planes detallados

| ID | Plan | Estado |
|----|------|--------|
| F79-A1 | [Atom matrix](1761000001000-f79-atom-matrix-and-migration-contract.md) | Hecho |
| F79-B1 | [Next Lit adapters](1761000002000-f79-next-lit-adapters.md) | Hecho |
| F79-B2 | [Ionic Lit adapters](1761000003000-f79-ionic-lit-adapters.md) | Hecho |
| F79-B3 | [RN token/API parity](1761000004000-f79-rn-token-api-parity.md) | Hecho |
| F79-C1 | Storybook adapter parity | → F80-D1 |
| F79-D1 | Arquetipos visual sameness | → F80-D1 |
| F79-E1 | CI gates & docs | → F80-D1 |

## Checklist de cierre F79

- [x] F79-A1 matriz publicada en docs + enlazada desde design-system
- [x] F79-B1 Next átomos SoT migrados (API pública estable / deprecations)
- [x] F79-B2 Ionic átomos SoT migrados (chrome Ion documentado)
- [x] F79-B3 RN props/tokens alineados; gaps explícitos
- [x] Seed layout `atoms|chrome|composites|theme` en next/ionic/rn (arranque F80)
- [x] C1/D1/E1 trasladados a F80-D1
- [x] Cambios commitidos
- [x] **F79 cerrada — F80 abierta**

## Predecesora / Siguiente

- Predecesora: [F78](../plans-78-seventy-eight-round/)
- Siguiente: [F80](../plans-80-eighty-round/) — UI folder layout, brand wrappers ↔ apps, dead-atom purge
