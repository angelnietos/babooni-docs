# F52-C1 — Alinear peerDependencies Angular / Ionic / React Native

## Estado

completado

## Resultado — matriz canónica

| Paquete | Peers |
|---------|--------|
| `@base/angular-ui` | `@angular/common\|core\|router` ^21, `rxjs` ^7.8 |
| `@base/native-ui` | (deps: `lit` — no peer; CE host) |
| `@base/ionic-ui` | `@angular/common\|core` ~21.2, `@ionic/angular` ^8.5 |
| `@base/react-native-ui` | `react`/`react-dom` **18.3.1**, `react-native` 0.76.9, `react-native-web` ~0.19 |

Cambios: ionic-ui y react-native-ui pasan frameworks a peers (antes deps).
Metro pin React 18 documentado en [add-mobile-domain.md](../../../guides/add-mobile-domain.md).
Smoke: `nx typecheck react-native-single` + `ionic-single` verdes.

## Criterios de aceptación

- [x] Matriz en Resultado + guía mobile.
- [x] Libs objetivo con peers coherentes.
- [x] Nota en add-mobile-domain + npm-publish.
