# F59-A2 — Piloto clients: reglas compartidas FE+BE

## Estado

trasladado → F60-E1 ([plans-60](../plans-60-sixty-round/))

## Objetivo

Aplicar A1 al dominio **clients** (kernel): Create/Update con **una** definición
de reglas usada por Nest y por al menos un formulario Angular plantilla
(`angular-single` / `@base/clients-features` o arquetipos) y un path React
equivalente si existe create form.

## Entregables

1. Reglas en shared (p. ej. `clientCreateRules` / schema): name required,
   email format, campos opcionales alineados a `CreateClientDto` /
   `UpdateClientDto`.
2. Backend: DTO o pipe sigue fallando igual (o mejor) — sin regresiones e2e/API.
3. FE Angular: form create/edit usa adapters de reglas shared (no
   `Validators.email` suelto duplicado).
4. FE React (si hay form): mismo schema/resolver.
5. Tests unit: shared rules + un smoke Nest ValidationPipe body inválido.
6. Documentar en guía de validación el piloto como plantilla para otros dominios.

## Criterios de aceptación

- [ ] Mismo input inválido → rechazo BE y error de form FE (campos alineados).
- [ ] `pnpm nx test` del shared/piloto verde.
- [ ] No regresiona login/clients smoke e2e existente.
- [ ] Josanz create puede **adoptar** en follow-up (no obligatorio en A2 si
      branding propio; al menos documentar gap).

## Depende de

F59-A1.

## Verificación

```bash
pnpm nx test base-shared -- --coverage
# body inválido contra API local o unit ValidationPipe
pnpm nx run angular-single-e2e:e2e -- --project=chromium   # smoke clients
```
