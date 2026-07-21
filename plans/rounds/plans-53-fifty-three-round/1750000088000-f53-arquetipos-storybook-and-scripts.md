# F53-D2 — Storybook arquetipos huérfanos + scripts root

## Estado

listo para ejecutar

## Objetivo

`libs/arquetipos/frontend/{angular,react}/ui/.storybook` existen con stories
pero **sin** targets Nx. O bien se cablean (`storybook` + `build-storybook`)
o se documenta “usar SB native + wrappers base” y se archivan configs.

Además: scripts root faltan (`storybook:native`, `storybook:base-angular`, …).

## Tareas

1. Decidir por plantilla arquetipos: **wire** vs **deprecate** (preferir wire
   ligero si hay stories de marca `ArqNative*`).
2. Añadir targets Nx coherentes con B1 (puertos 4404/4405).
3. Scripts en root `package.json` apuntando a los `nx storybook …`.
4. Actualizar design-system tabla de Storybooks (puerto / proyecto / dueño).

## Criterios de aceptación

- [ ] No quedan `.storybook` “huérfanos” sin target ni nota de deprecación.
- [ ] `pnpm storybook:*` documentados para native + base (+ arquetipos si wire).
