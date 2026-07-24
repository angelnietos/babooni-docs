<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F79-D1 — Arquetipos visual sameness</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F79" src="https://img.shields.io/badge/round-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Trasladado →** [F80-D1](../plans-80-eighty-round/1762000005000-f80-f79-carries-sb-ci.md) (F79 cerrada)

## Objetivo

Que las plantillas Arquetipos (Angular / React / Next / Ionic / RN × single/multi donde aplique) **consuman los mismos átomos base** (Lit SoT vía adapter, o twin RN), de modo que login/clients/shell se vean equivalentes salvo chrome de plataforma.

## Paths típicos

- UI thin: `libs/arquetipos/frontend/{angular,react,next}/ui/`, `…/mobile/{ionic,rn}/ui/`
- Features: `libs/arquetipos/frontend/**/features/`
- Apps: `apps/arquetipos/frontend/**`

## Reglas

1. Features Arquetipos **no** definen Button/Input/Alert locales.
2. Import path canónico por runtime:
   - Angular → `@arquetipos/arquetipos-angular-ui` (thin → `@base/angular-ui` / native)
   - React → `@arquetipos/arquetipos-react-ui` → `@base/react-ui` `Native*`
   - Next → `@arquetipos/…-next-ui` o `@base/next-ui` migrado
   - Ionic → `@base/ionic-ui` / thin arquetipos
   - RN → `@base/react-native-ui` / thin arquetipos
3. Product UI solo branding (re-export vs wrap — [ui-re-export-vs-wrapper.md](../../../guides/ui-re-export-vs-wrapper.md)).
4. Temas tenant/atmósfera: checklist existente; no hardcodear `--arq-brand` en hosts.

## Acciones

- [ ] Auditar imports de átomos en features Arquetipos (rg `button|ArqButton|ion-button|className=.*btn`)
- [ ] Sustituir forks por adapters migrados (por dominio: auth, clients primero)
- [ ] Verificar demos native-ui en cada app si existen
- [ ] Smoke visual manual o e2e smoke por stack (login + clients list)
- [ ] Actualizar [arquetipos-thin-libs.md](../../../frontend/arquetipos-thin-libs.md) con nota “UI atoms → Lit adapters”

## Criterios de aceptación

- [ ] Auth + clients (mínimo) usan adapters SoT en los 5 runtimes aplicables
- [ ] No hay CSS button one-off nuevo en features
- [ ] Builds canario verdes: angular/react/next/ionic (+ RN typecheck)

## Verificación

```bash
pnpm check:ui-ownership
pnpm nx run-many -t typecheck -p tag:layer:arquetipos
# smoke builds canarios documentados en F78-A5 / F79-E1
```
