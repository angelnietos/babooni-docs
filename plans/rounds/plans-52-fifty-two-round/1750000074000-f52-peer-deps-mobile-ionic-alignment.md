# F52-C1 — Alinear peerDependencies Angular / Ionic / React Native

## Estado

listo para ejecutar

## Objetivo

Apps mobile y libs `@base/ionic-*` / `@base/react-native-*` (y UI Angular)
arrastrán riesgos de **dual package** (React 18 vs 19, Angular peers sueltos,
Ionic sin peer declarado). F51-E1 / F52-A1 exigen peers correctos para
publicables; aunque no se publique mobile, alinear reduce pantallas blancas y
warnings de pnpm.

## Alcance

**Sí:**

- Inventario de `peerDependencies` / `devDependencies` en:
  - `@base/angular-ui`, `@base/native-ui`
  - `@base/ionic-ui` y shells/features ionic tocados
  - libs RN UI + Metro pin (React 18)
- Matriz versión canónica (tabla en Resultado): Angular X, RxJS, Ionic,
  `react`/`react-dom` 18.x para RN, 19.x para web raíz si aplica.
- Ajustes mínimos en `package.json` + `pnpm.overrides` solo si ya hay patrón
  en root (no inventar overrides agresivos).

**No:**

- Migrar Expo a React 19.
- Publicar paquetes Ionic a npm en esta tarea (eso es A1).

## Tareas

1. Listar warnings `peer dependency` relevantes (`pnpm install` / `pnpm why`).
2. Declarar peers en libs UI/mobile según matriz.
3. Verificar Metro pin documentado ([add-mobile-domain.md](../../../guides/add-mobile-domain.md)).
4. Smoke: `pnpm nx serve ionic-single` o typecheck proyectos mobile afectados.
5. Si A1 publica `@base/angular-ui`: peers deben coincidir con esta matriz.

## Criterios de aceptación

- [ ] Matriz de peers en Resultado (versiones canónicas).
- [ ] Libs objetivo con peers coherentes; sin dual React en bundle RN web
      (smoke o justificación).
- [ ] Nota en guía mobile o npm-publish si afecta consumidores externos.
