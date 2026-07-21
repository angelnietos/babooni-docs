# Ronda F53 — Native-ui SoT + Storybook serve + freeze framework UI

## Estado

completado

> Cerrado junto con F54 (wrappers 14+3 CEs, Storybook native, soft/strict gate,
> inventario A3). Detalle en [F54 Resultado](../plans-54-fifty-four-round/README.md).

## Dependencias externas

- F52 completada (publish workflow, workspace-deps, coverage soft, peers).
- Carry F52: coverage **strict**, ampliar oleada npm (planes D* aquí).
- Hoy: 14 CEs en native-ui; wrappers Angular/React solo para **3** (button/alert/input).
- Storybook: `build-storybook` en base-angular/react-ui; **serve** solo en
  `josanz-angular-ui`; **cero** SB en `base-native-ui`.

## Hitos

| ID | Plan | Origen |
|----|------|--------|
| F53-A1 | [Política native-first + freeze UI framework en base](1750000080000-f53-native-ui-sot-and-framework-freeze.md) | Producto / DX |
| F53-A2 | [Paridad wrappers + catálogo native-ui](1750000081000-f53-native-wrappers-parity.md) | SoT incompleta |
| F53-A3 | [Inventario e integración UI de proyectos externos](1750000082000-f53-import-external-ui-components.md) | Carry producto |
| F53-B1 | [Nx `storybook` serve en UI base (Angular/React)](1750000083000-f53-nx-storybook-serve-base-ui.md) | DX Storybook |
| F53-B2 | [Storybook de `@base/native-ui` (serve + build)](1750000084000-f53-native-ui-storybook.md) | SoT visual |
| F53-C1 | [Apps priorizan native + wrappers (canario)](1750000085000-f53-apps-prefer-native-wrappers.md) | Adopción |
| F53-C2 | [Gate soft: no nuevos primitivos solo-framework](1750000086000-f53-soft-gate-no-new-framework-primitives.md) | Gobernanza |
| F53-D1 | [Carry: coverage BE soft → strict](1750000087000-f53-coverage-ci-strict.md) | Carry F52-B2 |
| F53-D2 | [Storybook arquetipos huérfanos + scripts root](1750000088000-f53-arquetipos-storybook-and-scripts.md) | DX |
| F53-E1 | [Tokens / API conceptual native ↔ RN](1750000089000-f53-native-rn-token-alignment.md) | Mobile parity |
| F53-F1 | [Documentación y cierre F53](1750000090000-f53-documentation.md) | Cierre |

## Objetivo

1. **Dejar claro en biblia:** a partir de ahora no se mantienen (salvo bugs
   críticos) nuevos primitivos Angular/React/Ionic/RN **duplicados** en base;
   la SoT es `@base/native-ui` + wrappers delgados que capan/extienden.
2. Completar wrappers y Storybook del catálogo Lit.
3. Preparar import de componentes de otros proyectos (inventario, no dump ciego).
4. `pnpm nx storybook <ui-lib>` usable (serve), no solo `build-storybook`.
5. Apps arquetipos/clientes/SaaS **priorizan** native wrappers.
6. Carry hygiene (coverage strict) + tokens RN + gates soft.

## Orden de ejecución

1. **A1** (política/docs) — bloquea el resto culturalmente.
2. **B1 + B2** (Storybook) en paralelo a **A2** (wrappers).
3. **A3** inventario (puede ir en paralelo; import real puede partirse a F54).
4. **C1** canario apps tras A2 mínimo (button/alert/input ya existen).
5. **C2** gate soft tras A1.
6. **D1 / D2 / E1** en paralelo cuando basen.
7. **F1** cierre.

## Riesgos

- Congelar Angular/React UI sin wrappers suficientes → fricción en features.
- Lit en RN: **no** forzar CE; solo tokens/API (E1).
- Import externos: conflictos de marca / licencia / duplicados vs native.
- Storybook Lit + Angular en mismo CI: tiempo de build — canario native primero.

## Criterios de aceptación (ronda)

- [ ] Política freeze + native-first publicada en `ui-strategy` / design-system.
- [ ] `nx storybook base-native-ui` (o nombre fijado) + serve en base Angular/React UI.
- [ ] Wrappers Angular/React para **todos** los CEs canónicos (o exclusiones
      documentadas en Resultado A2).
- [ ] Inventario A3 con prioridad de import (aunque el código llegue en F54).
- [ ] Canario app(s) usando native wrappers documentado.
- [ ] Soft gate o checklist anti-primitivo-framework-nuevo.
- [ ] Hub: F53 activa o cerrada; F52 en archivo.
- [ ] `pnpm check:deprecated` + `pnpm check:exports-paths` (+ hygiene UI si aplica).

## Siguiente

Tras F53-F1 → [plans-54-fifty-four-round](../plans-54-fifty-four-round/)
(migración features, oleadas import, deprecation, CI SB/visual, tokens, a11y,
gate strict, publish UI, Figma).
