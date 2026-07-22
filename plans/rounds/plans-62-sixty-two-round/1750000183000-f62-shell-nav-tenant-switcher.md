# F62-B2 — Shell / nav / tenant switcher

## Estado

listo para ejecutar

## Objetivo

Pulir el chrome de apps arquetipos (sidebar, topbar, tenant switcher) — resto
explícito de F60-D1 / percepción “todo feo”. Hoy active state débil, switcher
tipo pill genérico, tipografía flat; el tema (A1) no se nota en el nav.

## Entregables

1. Estilos shell en `@base/ui-styles` `layout/` (`arq-shell*`, `arq-nav*`,
   `arq-tenant*`) — Tailwind utilities donde ayude; BEM para estructura.
2. Tenant switcher: trigger + panel con el **mismo** patrón listbox que
   F62-A2 (reutilizar `base-select` o menú BEM compartido) — **no** menú OS.
3. Active nav: barra + fondo usando vars de tema (A1); logo/brand coherente.
4. Angular + React multi/single alineados; Next/Ionic chrome si aplica
   (misma jerarquía visual).
5. Densidad / motion enter corta en shell (tokens motion).

## Criterios de aceptación

- [ ] Tenant switcher no abre menú nativo del browser.
- [ ] Active item claramente teñido por tema actual.
- [ ] Logout / branding no compiten con el contenido (jerarquía clara).
- [ ] Shell no introduce CSS de página de feature (solo layout chrome).

## Verificación

Manual multi-tenant + smoke e2e login → clients.

## Depende de

F62-A1, F62-A2 (patrón listbox), F62-A3 recomendado (botones chrome).

## Bloquea

Percepción global F62.
