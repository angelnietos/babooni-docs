# F60-D1 — Adoptar composiciones nativas en apps arquetipos

## Estado

listo para ejecutar

## Objetivo

Cablear `apps/arquetipos/**` para que **usen** las features nativas C1/C2 y
subir el listón de paridad (manifest + docs). Apps thin: solo bootstrap/routes.

## Apps

| App | Puerto tip. | Acción |
|-----|-------------|--------|
| angular-single / multi | 4200 / 4203 | features → composición nativa |
| react-single / multi | 4201 / 4202 | igual |
| next-single / multi | 4240 / 4241 | LoginForm/pages → composición |
| ionic-single / multi | 4300 / 4301 | login + clients shells |
| react-native-single / multi | 8091 / 8092 | espejo tokens + copy; documentar gaps |

## Entregables

1. Diff por app: imports a composición; borrar CSS duplicado de login/list.
2. Actualizar [parity.md](../../../arquetipos/parity.md) +
   [parity.manifest.yaml](../../../arquetipos/parity.manifest.yaml): elevar
   Next/Ionic hacia `full`/`core`+native donde aplique; RN sigue `token` con
   checklist explícito.
3. Scripts smoke ya existentes siguen verdes (`arq:e2e:all` local).
4. Guía corta: “cómo añadir una feature nativa nueva”.

## Criterios de aceptación

- [ ] Las 4 apps DOM single+multi usan C1 (login).
- [ ] Angular+React (+ ideal Next/Ionic) usan C2 clients.
- [ ] `check:arquetipos-parity` no empeora; strict si el manifest lo exige.
- [ ] RN: gaps listados, no silencio.

## Verificación

```bash
pnpm arq:typecheck:mobile-next
pnpm arq:fe:e2e:smoke          # SPA Keycloak
pnpm arq:fe:e2e:next
pnpm arq:mobile:e2e:smoke
pnpm check:arquetipos-parity
```

## Depende de

F60-C1, F60-C2.
