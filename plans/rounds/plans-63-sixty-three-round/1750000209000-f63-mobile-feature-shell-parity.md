# F63-C1 — Mobile feature parity + dual shell

## Estado

en progreso

## Problema

Ionic/RN no exponían las mismas features que Angular/React (`TEMPLATE_NAV`),
Ionic desktop no reutilizaba el shell SPA, y el chrome móvil no era compartido
entre Ionic y RN.

## Alcance

1. Rutas canónicas Ionic: `/clients`, `/users`, `/roles`, `/audit`, `/settings`,
   `/native-ui` (sin Dashboard primario).
2. Ionic ≥1024px: `lib-app-shell` (`@base/angular-ui`) = mismo chrome web.
3. Ionic &lt;1024px + RN: `@base/mobile-chrome` + `ArqMobileShell`.
4. Dominios Ionic 4 capas thin para users/roles/audit/settings.
5. RN AuthApp con mismos tabs de features.

## Criterios

- [x] Nav Ionic/RN alineada a `TEMPLATE_NAV_ITEMS`.
- [x] Desktop Ionic usa `arq-shell` / `lib-app-shell`.
- [x] Mobile chrome compartido (contrato tokens + IA).
- [ ] e2e smoke Ionic `/clients` verde en CI soft.
- [ ] `check:arquetipos-parity` documenta mobile routes.

## Dependencias

F63-A1 (select portal). Carry visual indigo → platform teal.
