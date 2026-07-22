<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F45-A1 — Eliminar deprecated remanentes `remove-next-minor`</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Remover o migrar los símbolos marcados como `remove-next-minor` en el inventario
de F44-A1 que siguen presentes en el código.

## Entradas

- `docs/plans/rounds/plans-44-forty-four-round/deprecations-inventory.md`
- Política de deprecación de F44-A2 (`docs/guides/deprecation-policy.md`)

## Símbolos a eliminar/migrar

| # | Archivo:linea | Símbolo | Acción |
|---|---------------|---------|--------|
| 1 | `libs/clientes/josanz/angular-ui/src/lib/catalog/josanz-catalog-list/josanz-catalog-list.ts:88` | `JosanzCatalogListConfig.idColumnLabel` | Eliminar propiedad |
| 2 | `libs/clientes/josanz/angular-ui/src/lib/catalog/josanz-catalog-list/josanz-catalog-list.ts:107` | `JosanzCatalogListConfig.showAdvancedFilters` | Eliminar propiedad |
| 3 | `libs/clientes/josanz/angular-ui/src/lib/catalog/josanz-catalog-list/josanz-catalog-list.ts:108` | `JosanzCatalogListConfig.showStatusFilters` | Eliminar propiedad |
| 4 | `libs/clientes/josanz/angular-ui/src/lib/layout/main-template-card/main-template-card.ts:60` | `MainTemplateCardComponent.avatarGradient` | Eliminar `@Input()` |
| 5 | `libs/productos-saas/verifactu/shared/core/crm/src/lib/domain/verifactu.repository.port.ts:66` | `VerifactuRepositoryPort.persistAeatChainHeadIfPresent` | Eliminar método |
| 6 | `libs/clientes/josanz/angular-ui/src/lib/services/theme.service.ts:25` | `JosanzListViewMode` | Eliminar type alias |
| 7 | `libs/clientes/josanz/angular-ui/src/lib/services/theme.service.ts:177` | `JosanzThemeService.setListViewMode` | Eliminar método |

## Tareas

1. **Verificar call sites activos**:
   - Para cada símbolo, buscar usos activos en el workspace.
   - Si hay usos en `@arquetipos/*` o productos SaaS, migrarlos antes de eliminar.
2. **Eliminar símbolos**:
   - Remover las propiedades/métodos/types del código.
   - Remover el JSDoc `@deprecated` asociado.
3. **Actualizar tests**:
   - Ajustar specs que referencien los símbolos eliminados.
4. **Verificar**:
   - `pnpm check:deprecated` pasa.
   - `pnpm nx typecheck` en proyectos afectados pasa.

## Restricción

No eliminar un símbolo si tiene call sites activos sin migrar. En ese caso,
marcar el call site como `// allowed-deprecated: <razón>` y dejarlo para F46.

## Criterios de aceptación

- [ ] Los 7 símbolos listados ya no existen en el código.
- [ ] `pnpm check:deprecated` pasa sin errores.
- [ ] Tests actualizados.

## Dependencias

- Ninguna.
