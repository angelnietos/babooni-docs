<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F79-C1 — Storybook adapter parity</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F79" src="https://img.shields.io/badge/round-F79-14b8a6?style=flat-square" /></a>
</p>

## Estado

**Trasladado →** [F80-D1](../plans-80-eighty-round/1762000005000-f80-f79-carries-sb-ci.md) (F79 cerrada)

## Objetivo

Que **todos** los Storybooks de adapters documenten el **mismo set de átomos SoT** que `base-native-ui`, sin catálogos “más pobres” en Next / Ionic / RN.

## Contrato (ADR 0011)

| Package | Port | Framework SB |
|---------|------|--------------|
| `base-native-ui` | 6007 | web-components-vite · Lit `html` · **SoT** |
| `base-angular-ui` | 4402 | Angular |
| `base-react-ui` | 4403 | react-vite |
| `base-next-ui` | 4406 | react-vite |
| `base-ionic-ui` | 4407 | Angular |
| `base-react-native-ui` | 4408 | react-vite + RN-web |

Scripts: `pnpm storybook:native-ui|base-angular|base-react|base-next|base-ionic|base-rn`.

## Definición de “paridad”

Para cada átomo SoT en la matriz:

1. Existe story en `base-native-ui` (ya).
2. Existe story en cada adapter **donde el átomo aplica** (RN puede marcar N/A).
3. Stories de adapter muestran el **wrapper real** (no re-embed del CE Lit en SB React salvo que el wrapper sea el CE).
4. Controls mínimos alineados: variant / size / disabled / label (según API).
5. Chrome-only (Ion tabs, etc.) = sección separada `Chrome/` — no cuenta como cobertura SoT.

## Acciones

- [ ] Inventariar stories actuales por paquete vs lista Lit (~23)
- [ ] Completar gaps Next (hoy ~5), Ionic (~3), RN (~3)
- [ ] Asegurar targets `storybook` + `build-storybook` en `project.json` (ya parcialmente)
- [ ] Preview: tokens `--arq-*` cargados en todos los adapters
- [ ] Título/organización Storybook: `Atoms/<Name>` consistente
- [ ] (Opcional) checklist CI: fail soft si falta story para átomo matriculado

## Criterios de aceptación

- [ ] Diff matriz: 0 gaps “missing story” en Next/Ionic/RN salvo N/A documentado
- [ ] `pnpm nx run base-native-ui:build-storybook` + adapters build verde
- [ ] Docs design-system matriz SB actualizada (sin “No Storybook” para Next/Ionic/RN)

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
pnpm nx run base-next-ui:build-storybook
pnpm nx run base-ionic-ui:build-storybook
pnpm nx run base-react-native-ui:build-storybook
# angular/react ya existentes
```
