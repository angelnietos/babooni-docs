<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F66-A2 — Entity view abstractions (read / write)</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F66" src="https://img.shields.io/badge/round-F66-14b8a6?style=flat-square" /></a>
</p>

## Estado

listo para ejecutar

## Objetivo

Introducir **clases / hooks padre genéricos** para vistas de entidad de forma
que, por extensión o configuración, se obtengan:

- Vista **solo lectura** (detalle / audit).
- Vista **solo escritura** (create / edit) de un subconjunto de propiedades.
- Vista **mixta** (algunos campos read-only, otros editables) sin duplicar
  templates.

Patrón: **Template Method** + configuración de campos (`FieldAccess`), no
copiar-pegar formularios por dominio.

## Entregables

### A2.1 — Contrato

Documentar en [entity-view-abstractions.md](../../../frontend/entity-view-abstractions.md):

```ts
type FieldAccess = 'read' | 'write' | 'hidden';

interface EntityViewConfig<T> {
  mode: 'read' | 'write' | 'mixed';
  fields: Partial<Record<keyof T, FieldAccess>>;
}
```

- **Angular / Ionic:** clase abstracta o base component
  (`EntityFormViewBase<T>`) con `config`, `value`, `onSubmit`, helpers
  `isWritable(key)` / `isReadable(key)`.
- **React / Next / RN:** `useEntityView<T>(config)` + presentational
  `EntityField` / layout; o clase TS + hook fino.
- Subclases de dominio: `ClientsFormView extends EntityFormViewBase<ClientDto>`
  solo define `config` + labels / slots UI.

### A2.2 — Piloto `clients`

1. Detail read-only: `email` + `name` read; acciones (edit/delete) fuera.
2. Edit: `name`/`email` write; `id`/`tenantId` read o hidden.
3. Create: solo campos write del create DTO.

Consumir desde features (A1): el panel llama al facade; la vista padre no hace
HTTP.

### A2.3 — Ubicación de código

Preferir `@base/angular-ui` / `@base/react-shared` o helper en
`@base/angular-store` / paquete crosscutting **sin** acoplar a un dominio.
Dominio solo extiende.

## Criterios de aceptación

- [ ] Guía + tipos públicos documentados.
- [ ] Piloto clients (Angular **y** React) con vista read y write por
      propiedad (tests de `isWritable` / render).
- [ ] Al menos un ejemplo de subclase/extension en arquetipos o base features.
- [ ] No introduce segunda fuente de verdad de validación (sigue predicates /
      ADR 0012).

## Verificación

```bash
pnpm nx typecheck base-angular-clients-features
pnpm nx typecheck base-react-clients-features
pnpm nx test base-angular-clients-features
pnpm nx test base-react-clients-features
```

## Depende de

F66-A1 (páginas thin). F65-D1 facades. ADR 0006.

## Fuera de alcance

- Form builders visuales / JSON Schema completo.
- Generación automática de UI desde OpenAPI en esta ronda.
