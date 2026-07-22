# F63-A2 — Tenant switcher UX

## Estado

listo para ejecutar

## Objetivo

Pulir el switcher del header: label que no recorta, trigger denso, teclado,
feedback visual al cambiar tenant (sin recargar a ciegas).

## Entregables

1. Label responsive (ocultar &lt;900px); select min/max width estables.
2. `aria-*` + focus ring con tokens tenant.
3. Opcional: toast/eyebrow “Tema: Helix Labs” 1.2s al cambiar (no modal).

## Criterios de aceptación

- [ ] No se lee “Tenan…” cortado en desktop.
- [ ] Teclado Arrow/Enter/Escape operan el listbox.
- [ ] Cambio de tenant re-pinta shell en &lt;100ms percibido.

## Verificación

Manual multi A/R + smoke.

## Depende de

F63-A1.
