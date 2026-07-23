<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F75-D1 — Automated Validations & CI Gates</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F75" src="https://img.shields.io/badge/round-F75-14b8a6?style=flat-square" /></a>
</p>

## Estado

Cerrado

## Objetivo

Implementar verificaciones automáticas en CI y scripts de gobernanza para validar que ningún Pull Request ni agente de IA viole los límites arquitectónicos del contrato F75.

## Reglas Automáticas Auditadas en CI (`tools/ci/check-frontend-conventions.mjs`)

| ID | Regla | Descripción | Acción de CI ante Violación |
|----|-------|-------------|----------------------------|
| **F75-RULE-1** | `api` no importa `data-access` | La capa `api` no debe depender de nada relacionado con estado, store o data-access. | **ERROR (Build Failure)** |
| **F75-RULE-2** | `domain` es 100% puro | Ningún archivo en `domain/` puede importar `@angular/*`, `react`, `next`, `ionic`, `rxjs` o `axios`. | **ERROR (Build Failure)** |
| **F75-RULE-3** | `ui` no realiza HTTP | Ningún componente en `ui/` puede importar de `api/`, `axios`, `fetch` o `HttpClient`. | **ERROR (Build Failure)** |
| **F75-RULE-4** | `features` no llama a `api` directo | Los componentes/servicios de `features/` deben consumir `data-access/`, no importar directamente de `api/`. | **WARN / ERROR** |
| **F75-RULE-5** | `shell` solo depende de `features`, `ui` y `shared` | `shell/` no puede importar directamente `data-access/` ni `api/`. | **ERROR (Build Failure)** |
| **F75-RULE-6** | Prohibida duplicación de DTOs | Ninguna librería `api/` local puede declarar interfaces de DTO que ya existan en `@base/shared`. | **ERROR (Build Failure)** |
| **F75-RULE-7** | Prohibidos "God Barrels" y reexports transversales | Ningún `index.ts` puede contener `export * from '@base/*'` hacia otras librerías sin el tag deprecation TODO F75. | **ERROR (Build Failure)** |
| **F75-RULE-8** | Test y Lint targets obligatorios | Toda librería debe contar con targets `"test"` y `"lint"` válidos en su `project.json`. | **ERROR (Build Failure)** |

## Scripts de Comprobación Integrados

```bash
# Ejecutar verificación de gobernanza frontend F75
node tools/checks/check-frontend-conventions.mjs

# Ejecutar verificación de contratos de dominio F75
node tools/checks/check-domain-conventions.mjs

# Ejecutar auditoría de Public API Barrels (Prohibición de God Barrels)
node tools/checks/check-public-api-barrels.mjs
```

## Configuración ESLint (Boundary Check)

Se añade la siguiente regla de `eslint-plugin-boundaries` en `.eslintrc.base.json`:

```json
{
  "rules": {
    "@nx/enforce-module-boundaries": [
      "error",
      {
        "depConstraints": [
          {
            "sourceTag": "type:domain",
            "onlyDependOnLibsWithTags": ["type:shared"]
          },
          {
            "sourceTag": "type:api",
            "onlyDependOnLibsWithTags": ["type:domain", "type:shared"]
          },
          {
            "sourceTag": "type:data-access",
            "onlyDependOnLibsWithTags": ["type:api", "type:domain", "type:shared"]
          },
          {
            "sourceTag": "type:features",
            "onlyDependOnLibsWithTags": ["type:data-access", "type:ui", "type:domain", "type:shared"]
          },
          {
            "sourceTag": "type:shell",
            "onlyDependOnLibsWithTags": ["type:features", "type:ui", "type:shared"]
          },
          {
            "sourceTag": "type:ui",
            "onlyDependOnLibsWithTags": ["type:domain", "type:shared"]
          }
        ]
      }
    ]
  }
}
```
