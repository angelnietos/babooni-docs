<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Sonar — análisis local y alineación con CI</h1>

<p align="center">
  <img alt="guide" src="https://img.shields.io/badge/guide-14b8a6?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Herramientas

| Herramienta | Uso |
|-------------|-----|
| **SonarLint** (IDE) | Reglas en tiempo real sobre `apps/`, `libs/`, `tools/{checks,smoke,dx,…}/` |

| **SonarScanner** (opcional) | `sonar-scanner` con `sonar-project.properties` en la raíz |

## Alcance

`sonar-project.properties` excluye:

- `**/generated/**` (Prisma, etc.)
- `**/*.spec.ts`, `**/*.stories.ts`
- `node_modules`, `dist`, `coverage`

## Scripts de smoke (F13)

Los scripts en `tools/smoke/*verifactu*.mjs` y `tools/dx/preflight-dev-infra.mjs` siguen estas convenciones para evitar hallazgos Sonar:

| Regla | Mitigación |
|-------|------------|
| **S7785** top-level await | Usar `await` a nivel módulo en `.mjs` (Node ESM) |
| **S4036** PATH en comandos OS | `docker` vía `execFile('docker', [...])`; health checks en contenedor con `docker compose exec` |
| **S4030** colecciones sin uso | No acumular en arrays si solo se hace `console.log` |
| **S7763** re-export | `export { x } from './mod'` en barriles TypeScript |

Supresión documentada en `sonar-project.properties` solo para **S7785** en `tools/**` si el servidor Sonar no reconoce top-level await en ESM.

## Verificación manual

```bash
node tools/dx/preflight-dev-infra.mjs
node tools/smoke/smoke-verifactu-worker.mjs
node --check tools/smoke/run-verifactu-smoke.mjs
```

## CI (opcional F14)

Job dedicado con servicio Redis + `pnpm saas:worker:smoke` — no mezclar con `verify` principal (requiere Docker service).

## Relación con ESLint

Module boundaries y UI ownership siguen en ESLint (`pnpm check:ui-ownership`, `pnpm hygiene`). Sonar complementa calidad de scripts y duplicación en libs.
