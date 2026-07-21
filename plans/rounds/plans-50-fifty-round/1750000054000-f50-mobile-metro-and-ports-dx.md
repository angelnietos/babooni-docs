# F50-C1 — DX mobile Metro + matriz de puertos

## Estado

listo para ejecutar

## Objetivo

El pin React 18 en Metro (evitar React 19 hoisted) está duplicado en
`react-native-single` y `react-native-multi`. Extraer helper compartido y
asegurar que [local-development.md](../../../guides/local-development.md) /
[add-mobile-domain.md](../../../guides/add-mobile-domain.md) coinciden con la realidad.

## Tareas

1. Extraer `apps/arquetipos/mobile/metro.shared.cjs` (o path bajo `tools/`) con
   `disableHierarchicalLookup` + `extraNodeModules` React 18.
2. Ambos `metro.config.js` lo consumen.
3. Smoke: `pnpm nx serve react-native-single` — login visible (no pantalla blanca).
4. Verificar matriz de puertos en docs vs `project.json` (Ionic 4300/4301, RN 8091/8092, Next 4240, SaaS 3120/4230…).

## Criterios de aceptación

- [ ] Un solo módulo shared de Metro para single + multi.
- [ ] Docs de puertos sin contradicciones.
- [ ] Smoke RN web login OK (nota en Resultado).
