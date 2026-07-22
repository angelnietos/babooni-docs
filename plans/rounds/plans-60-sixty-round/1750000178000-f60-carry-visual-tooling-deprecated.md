# F60-E2 — Carry F59: Chromatic / Code Connect / `@deprecated`

## Estado

listo para ejecutar

## Objetivo

Cerrar o deferir con bloqueo explícito los carries visuales/tooling de F58→F59:

| F59 | Título |
|-----|--------|
| [B1](../plans-59-fifty-nine-round/1750000164000-f59-chromatic-or-visual-alt.md) | Chromatic o alternativa |
| [B2](../plans-59-fifty-nine-round/1750000165000-f59-figma-code-connect-pilot.md) | Code Connect pilot |
| [B3](../plans-59-fifty-nine-round/1750000166000-f59-remove-deprecated-primitives.md) | Remove `@deprecated` primitives |

## Entregables

1. **B1:** `CHROMATIC_PROJECT_TOKEN` + publish SB native-ui **o** alternativa
   ( Percy / captura CI / nightly `build-storybook` soft) **o** defer F61 en
   `design-system.md` + `ci-gates.md`.
2. **B2:** `*.figma.ts` para `base-button`, `base-input`, `base-alert` **o**
   defer F61 (sin acceso Figma).
3. **B3:** inventario consumers → migrar a `Native*` / composición F60 **o**
   lista residual + defer F61. Priorizar login/audit/stories que bloquean freeze
   ADR 0010.

## Criterios de aceptación

- [ ] Cada B1/B2/B3: hecho **o** defer F61 con motivo (token/acceso/consumers).
- [ ] No dejar “listo para ejecutar” eterno sin owner.

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
# + Chromatic CLI si token presente
rg -n "@deprecated" libs/base/frontend --glob "*.ts" | head
```

## Depende de

F60-B1 recomendado antes de Chromatic; B3 se beneficia de C1/C2/D1.
