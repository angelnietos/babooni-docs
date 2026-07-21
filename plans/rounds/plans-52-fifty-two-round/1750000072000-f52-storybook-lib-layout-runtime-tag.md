# F52-B1 — Storybook Josanz: runtime tag + lib-layout warn

## Estado

listo para ejecutar

## Objetivo

`node tools/scripts/check-lib-layout.mjs` deja **1 warn**:

```
warn libs/clientes/josanz/storybook: could not infer runtime category from path
```

Tooling Storybook no es un dominio de 4 capas; hay que etiquetarlo de forma
explícita (tag `runtime:*` + excepción documentada o path/heurística) para que
`--strict` / CI no acumule ruido.

## Tareas

1. Revisar cómo `check-lib-layout.mjs` infiere `runtime:*` y excepciones
   existentes (`platform-no-features`, etc.).
2. Elegir una de:
   - **A)** Añadir excepción documentada `josanz-storybook-tooling` + tag
     `runtime:angular` (o `type:tooling` si el script lo soporta).
   - **B)** Ajustar heurística de path para `…/storybook` bajo clientes.
3. Actualizar `package.json` / `project.json` de `@josanz/storybook` con tags
   Nx coherentes (`layer:clientes`, runtime).
4. Mencionar en [design-system.md](../../../frontend/design-system.md) o
   [josanz-product-exceptions.md](../../../frontend/josanz-product-exceptions.md)
   que Storybook es tooling, no dominio.

## Criterios de aceptación

- [ ] `check-lib-layout` (warn mode): 0 warns storybook.
- [ ] Con `--strict` (si aplica a este caso): no fail por storybook.
- [ ] Excepción o heurística documentada en Resultado + doc frontend corta.
