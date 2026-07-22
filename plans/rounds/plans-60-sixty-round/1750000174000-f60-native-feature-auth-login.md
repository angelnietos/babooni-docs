<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F60-C1 — Feature nativa Auth login</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Una **composición de login** compartida (hero + form + demo users + estados de
error/conexión) construida solo con `@base/native-ui` / wrappers, consumible
por Angular, React, Next e Ionic. RN: pantalla espejo con mismos tokens/copy
(API conceptual), no Lit.

Hoy cada stack tiene markup distinto
(Angular/React arquetipos auth features, Next `LoginForm`, Ionic `login.page`,
RN `AuthApp`).

## Entregables

1. Decidir home de la composición (recomendado):
   - Opción A: lib features crosscutting bajo
     `libs/base/frontend/crosscutting/` o
   - Opción B: package `@base/native-auth-ui` / composición en native-ui
     `compositions/`.
   Documentar en A1/F1 la elección (una sola).
2. API estable: props/slots para título, subtítulo, multi-tenant switcher,
   submit, mensajes error, `apiUnavailable`.
3. Migrar **Angular + React** arquetipos auth features a la composición.
4. Next + Ionic: adoptar la misma composición DOM (o wrapper mínimo).
5. RN: alinear copy/layout/tokens; checklist de paridad visual (no pixel-perfect
   HTML).
6. Regresión e2e: `angular-*-e2e`, `react-*-e2e`, `next-*-e2e`, `ionic-*-e2e`
   verdes.

## Criterios de aceptación

- [ ] Login Angular y React se ven sustancialmente iguales (misma jerarquía /
      átomos).
- [ ] Next/Ionic usan native-ui (no HTML crudo de botón/input).
- [ ] Errores visibles (Keycloak down, credenciales, bounce) siguen UX F59.
- [ ] Sin `type=submit` Shadow DOM sin handler `clicked`/`onClick`.

## Verificación

```bash
pnpm nx run angular-single-e2e:e2e -- --project=chromium
pnpm nx run react-single-e2e:e2e -- --project=chromium
pnpm arq:fe:e2e:next
pnpm nx run ionic-single-e2e:e2e -- --project=chromium
```

## Depende de

F60-A2.

## Bloquea

F60-D1 (parcial).
