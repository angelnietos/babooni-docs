# F62-D2 — Chromatic / Code Connect / `@deprecated` (carry F60-E2)

## Estado

defer F63

## Objetivo

Cerrar el tooling visual deferido desde F58→F59→F60:

| Origen | Título |
|--------|--------|
| [F59-B1](../plans-59-fifty-nine-round/1750000164000-f59-chromatic-or-visual-alt.md) | Chromatic o alternativa |
| [F59-B2](../plans-59-fifty-nine-round/1750000165000-f59-figma-code-connect-pilot.md) | Code Connect pilot |
| [F59-B3](../plans-59-fifty-nine-round/1750000166000-f59-remove-deprecated-primitives.md) | Remove `@deprecated` primitives |
| [F60-E2](../plans-60-sixty-round/1750000178000-f60-carry-visual-tooling-deprecated.md) | Carry empaquetado |

## Entregables

1. **Chromatic (B1):** `CHROMATIC_PROJECT_TOKEN` + publish Storybook
   `base-native-ui` **o** alternativa (Percy / captura CI / nightly
   `build-storybook` soft en CI) **o** defer F63 con motivo en
   `design-system.md` + `ci-gates.md`.
2. **Code Connect (B2):** `*.figma.ts` piloto para `base-button`, `base-input`,
   `base-alert` (y `base-select` si A2 listo) **o** defer F63 (sin acceso Figma).
3. **Deprecated (B3):** inventario consumers → migrar a `Native*` / composiciones
   F60/F62 **o** lista residual + defer F63. Priorizar features arquetipos y
   stories que bloquean freeze ADR 0010.

## Criterios de aceptación

- [ ] Cada B1/B2/B3: hecho **o** defer F63 con motivo (token/acceso/consumers).
- [ ] No dejar “listo para ejecutar” eterno sin owner.

## Verificación

```bash
pnpm nx run base-native-ui:build-storybook
# + Chromatic CLI si token presente
rg -n "@deprecated" libs/base/frontend --glob "*.ts" | head
```

## Depende de

F62-D3 (stories estables); F62-A3 recomendado.

## Bloquea

Nada crítico de demo local.
