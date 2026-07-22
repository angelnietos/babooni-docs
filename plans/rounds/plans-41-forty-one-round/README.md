<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Plans � cuadrag�sima primera ronda (F41: deduplicaci�n + unificaci�n de convenciones)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="../../../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>


**Creado:** 2026-07-19  
**Estado:** Completada � F41-B1 a F41-F3 aplicados; verificaci�n completa cerrada
**Precedente:** [F40](../plans-40-forty-round/README.md) (polish bases; F40-B5 base repo `thin + include` + lint clean ?)

> Objetivo: eliminar la deuda de duplicaci�n y las convenciones divergentes detectadas en auditor�a de `libs/base`.
>
> Tema ra�z: coexisten **dos plantillas de dominio** en backend (thick vs thin) sin criterio claro, y **m�ltiples sistemas de estado** en frontend (Angular: NgRx + signals; React: RTK thunks + react-query + useCrud). Adem�s hay abstracciones v�lidas usadas a medias (`CrudService`, `providePrismaRepository`) y helpers de test copiados por dominio. Unificar en **una sola convenci�n** por capa resuelve la mayor�a de hallazgos a la vez.

## Deuda t�cnica a cerrar

### Backend (`libs/base/backend`)

| # | Deuda | Plan | Prioridad |
|---|-------|------|-----------|
| 1 | Handlers CRUD CQRS duplicados byte-a-byte en 5 dominios thin | F41-B1 | Alta |
| 2 | Fachadas `<domain>.service.ts` casi id�nticas + 2 nomenclaturas (`get/list` vs `findById/findPage`) | F41-B2 | Alta |
| 3 | Wiring de m�dulos copy-paste: 3 formas de registrar repo, `as unknown as`, contribuci�n AI repetida | F41-B3 | Alta |
| 4 | `findPage?` opcional en el port genera fallback de paginaci�n duplicado en 7+ sitios; `TenantlessPrismaRepository` no satisface el port (`as unknown as`) | F41-B4 | Media |
| 5 | Helpers de test duplicados (`loadCtx`/`buildRepo`/`Delegate`/`overrideGuard`) en ~16 specs | F41-B5 | Media |
| 6 | Mappers `toXxxUpdate`/`toXxxDto` repetidos; `as never` en repos; import `PRISMA_SERVICE` inconsistente | F41-B6 | Media |

### Frontend (`libs/base/frontend`)

| # | Deuda | Plan | Prioridad |
|---|-------|------|-----------|
| 7 | Sistemas de estado solapados (Angular: doble estado en `clients`; React: 3 APIs) sin convenci�n documentada | F41-F1 | Alta |
| 8 | Hooks `useX`/fetch `authorizedFetch` copiados literalmente en ~8 archivos React | F41-F2 | Media |
| 9 | C�digo demo en `layer:base` (`ClientsService.sumar`, `clients-math.ts`); `any` en specs; setup de specs repetido | F41-F3 | Media |

## Progreso por plan

| ID | Documento | Prioridad | Estado |
|----|-----------|-----------|--------|
| F41-B1 | [1784801000000-f41-crud-handler-factories.md](./1784801000000-f41-crud-handler-factories.md) | Alta | Completado |
| F41-B2 | [1784802000000-f41-unify-domain-service-facades.md](./1784802000000-f41-unify-domain-service-facades.md) | Alta | Completado |
| F41-B3 | [1784803000000-f41-module-wiring-helpers.md](./1784803000000-f41-module-wiring-helpers.md) | Alta | Completado |
| F41-B4 | [1784804000000-f41-align-repository-port.md](./1784804000000-f41-align-repository-port.md) | Media | Completado |
| F41-B5 | [1784805000000-f41-backend-test-helpers.md](./1784805000000-f41-backend-test-helpers.md) | Media | Completado |
| F41-B6 | [1784806000000-f41-mapper-and-cast-cleanup.md](./1784806000000-f41-mapper-and-cast-cleanup.md) | Media | Completado |
| F41-F1 | [1784807000000-f41-frontend-state-strategy.md](./1784807000000-f41-frontend-state-strategy.md) | Alta | Completado |
| F41-F2 | [1784808000000-f41-react-fetch-dedup.md](./1784808000000-f41-react-fetch-dedup.md) | Media | Completado |
| F41-F3 | [1784809000000-f41-remove-demo-and-harden.md](./1784809000000-f41-remove-demo-and-harden.md) | Media | Completado |

## Orden recomendado

1. **F41-F1** � Decidir la convenci�n de estado FE ANTES de deduplicar (la dedup de F41-F2/F3 depende de la decisi�n).
2. **F41-B4** � Alinear el port `Repository` (hacer `findPage` obligatorio, arreglar tenantless) porque desbloquea B1/B2 (elimina el fallback duplicado de ra�z).
3. **F41-B1** ? **F41-B2** ? **F41-B3** � Deduplicar handlers, fachadas y wiring (dependen de la convenci�n unificada).
4. **F41-B5** / **F41-B6** � Limpieza de tests, mappers y casts (independientes, bajo riesgo).
5. **F41-F2** / **F41-F3** � Deduplicaci�n y limpieza frontend.

## Principios de esta ronda

- **Una convenci�n por capa.** No dos plantillas de dominio ni tres sistemas de estado.
- **No romper API p�blica** de `@base/*` sin documentarlo; si cambia, actualizar los productos consumidores (`crm-*`, `josanz-*`).
- **Cero regresi�n de seguridad multi-tenant (F37-H1).** Toda dedup de repos/handlers debe conservar el tenant scoping.
- **Reducir archivos, no esconder complejidad.** Preferir borrar boilerplate a crear indirecciones nuevas.

## Verificaci�n al cerrar

- [x] Typecheck (lib+spec) verde en `base-backend`, libs frontend tocadas
- [x] Lint: 0 errores en `libs/base` (2 warnings preexistentes en kafka.event-bus)
- [x] Tests verdes en `base-backend` y libs frontend tocadas (109 suites, 304 tests)
- [ ] `pnpm check:domain-conventions:strict` verde
- [ ] Productos consumidores (`crm-*`, `josanz-*`) compilan si se toc� API p�blica de `@base/*`
- [ ] Doc de convenci�n actualizada: `docs/frontend/workspace-packages.md`
