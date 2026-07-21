# F53-C1 — Apps priorizan native + wrappers (canario)

## Estado

listo para ejecutar

## Objetivo

Apps arquetipos / clientes / SaaS deben **priorizar** `@base/native-ui` vía
wrappers (`@base/angular-ui` Native*, `@base/react-ui`, marca Arq/Josanz) en
lugar de primitivos framework-only o HTML suelto.

No migrar todo el monorepo en F53: **canarios** + guía de adopción.

## Tareas

1. Elegir canarios (sugerido):
   - `ionic-single` demo page (ya usa CE crudo) → wrappers o doc “CE OK en Ionic”.
   - Una feature Angular arquetipos (clients list) y/o React single: sustituir
     1–2 usos de Button framework por `NativeButton` / `ArqNativeButton`.
2. Checklist PR: “¿hay primitivo equivalente en native? → usarlo”.
3. Actualizar [add-frontend-domain.md](../../../guides/add-frontend-domain.md)
   o guía UI: default import path.
4. Josanz: no forzar migración masiva; documentar que **nuevas** pantallas
   usan wrappers sobre CE cuando existan (A2).

## Criterios de aceptación

- [ ] ≥ 1 app/feature canario migrada o demostrada.
- [ ] Guía “qué importar” enlazada desde hub / ui-strategy.
- [ ] Sin romper typecheck/lint de canarios.
