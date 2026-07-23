<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F72-C1 — Feature scaffold parity (multi-framework)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F72" src="https://img.shields.io/badge/round-F72-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Garantizar que **todos los frameworks** (Angular, React, Next, Ionic, RN) usan
el mismo scaffold canonico de `features/` respetando la arquitectura comun.

### Contrato comun de features (SoC + F66-A1)

```
features/src/
├── layout/        # chrome del dominio (título, outlet, wrappers de sección)
├── pages/         # targets de ruta — thin; componen layout + components
├── components/    # smart/presentacional; bindings a servicio/hook/facade
├── services/      # Angular / Ionic — orquestación de panel (opcional)
├── hooks/         # React / Next / RN — orquestación de panel (opcional)
├── *.routes.ts[x] # Angular/Ionic: rutas; React: barrel rutas si aplica
└── index.ts[x]    # barrel público del feature
```

**Reglas:**

1. `layout/`, `pages/`, `components/` son obligatorios en **todos** los frameworks.
2. La lógica de orquestación del panel vive en `services/` (Angular/Ionic)
   o `hooks/` (React/Next/RN). No en componentes.
3. `api/`, `data-access/`, `shell/` viven **al mismo nivel** de `features/`
   dentro del dominio (`{domain}/{api,data-access,shell,features}`).
4. Next/RN: si el stack no usa lazy `features/` cargado por `shell/`, conservar
   el mismo scaffold interno (`pages/`, `components/`, `hooks/`) para paridad
   mental.

### Estado actual (2026-07-23)

| Stack / scope | layout | pages | components | services | hooks | Gap |
|---------------|--------|-------|------------|----------|-------|-----|
| base-angular | ✓ | ✓ | ✓ | — | — | Falta `services/` cuando hay orquestación |
| base-react | ✓ | ✓ | ✓ | — | ✓ | OK (algunos hooks sueltos en components/) |
| base-ionic | ✓ | ✓ | ✓ | parcial | — | Falta `services/` en users/roles/auth/audit |
| arquetipos-angular | ✓ | ✓ | ✓ | — | — | Falta `services/` cuando haya panel orquestado |
| arquetipos-react | ✓ | ✓ | ✓ | — | — | Falta `hooks/` para orquestación |
| arquetipos-ionic | — | — | — | — | — | Sin scaffold de dominio en mobile |
| base-next | — | — | — | — | — | Sin dominio features estructurado |
| base-rn | — | — | — | — | — | Sin dominio features estructurado |
| arquetipos-next | — | — | — | — | — | Sin dominio features estructurado |
| arquetipos-rn | — | — | — | — | — | Sin dominio features estructurado |

### Entregables

1. Gate `check-frontend-conventions.mjs` actualizado para validar el scaffold
   canonico por framework en `base/` + `arquetipos/`.
2. Migración de features existentes que violan el contrato (p. ej. hooks
   dentro de `components/`, `services/` ausentes en Ionic).
3. Scaffolding base para Ionic arquetipos (clients, auth, users, roles, audit,
   settings) o defer F73 con owner.
4. Documentar excepciones Next/RN si aplica.

## Criterios de aceptación

- [ ] Nuevos dominios creados con `nx g @nx/angular:lib …` (o equivalente)
      incluyen el scaffold canonico automáticamente.
- [ ] `check-frontend-conventions.mjs` pasa en base + arquetipos para todos
      los frameworks con features.
- [ ] No hay hooks sueltos en `components/` (React) ni servicios de panel
      escritos inline en componentes (Angular/Ionic).
- [ ] Diferencias Next/RN documentadas o cerradas.

## Verificación

```bash
node tools/checks/check-frontend-conventions.mjs
pnpm typecheck:all
pnpm check:lib-layout
```

## No objetivo

- Migrar Next/RN si el stack no usa features lazy-cargados por shell, siempre
  que respeten el scaffold interno pages/components/hooks.
- Cambiar lógica de dominio que ya vive en `data-access/`.
