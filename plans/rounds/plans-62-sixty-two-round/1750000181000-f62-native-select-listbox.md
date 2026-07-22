# F62-A2 — Native Select listbox (matar `<select>` default)

## Estado

listo para ejecutar

## Objetivo

Sustituir el `<select>` HTML (menú OS azul, sin tema, “horrible”) por
`base-select` Lit con listbox propio (trigger + panel + opciones BEM),
alineado a tokens F60/F62.

Hoy `base-select` existe pero features arquetipos (users/roles React) siguen
usando `<select>` nativo; Angular a veces `ArqSelect` framework.

## Entregables

1. Polish visual/UX de
   [`base-select`](../../../../libs/base/frontend/crosscutting/native-ui/src/lib/select/):
   trigger, chevron, panel, hover/focus/selected, empty, disabled, teclado
   (↑↓ Enter Esc), a11y `listbox`/`option`.
2. Wrappers Angular/React ya existentes (`ArqNativeSelect` / `NativeSelect`)
   alineados; Storybook story de estados.
3. Migrar **users** y **roles** (Angular + React) a Native Select.
4. Gate soft opcional: `rg '<select'` en `libs/arquetipos/**/features` → 0 en
   camino feliz (salvo tests).

## Criterios de aceptación

- [ ] Panel de opciones con look de producto (no menú OS).
- [ ] Users: cambio de rol usa Native Select; e2e/smoke no regresan.
- [ ] Roles: mismos selects nativos.
- [ ] Story `base-select` en `base-native-ui`.

## Verificación

```bash
pnpm nx test base-native-ui
pnpm nx run base-native-ui:build-storybook
pnpm exec playwright test --config=apps/arquetipos/frontend/angular/multi-tenant/angular-multi-e2e/playwright.config.mts --project=chromium
```

## Depende de

F62-A1 recomendado (vars de tema en panel).

## Bloquea

F62-B1, F62-C2.
