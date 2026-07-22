<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Entity view abstractions — read / write por campo</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
  <a href="../plans/rounds/plans-66-sixty-six-round/1750000231000-f66-entity-view-abstractions.md"><img alt="F66-A2" src="https://img.shields.io/badge/F66--A2-14b8a6?style=flat-square" /></a>
</p>

Patrón objetivo (F66-A2): **vista de entidad domain-agnóstica** + config/extensión
por dominio. **No** es un layout de clients — el chrome de lista es
[feature-shell-presentation.md](./feature-shell-presentation.md) (A3).

## Contrato (borrador)

```ts
/** Access per field. `hidden` omits the control entirely. */
export type FieldAccess = 'read' | 'write' | 'hidden';

export type EntityViewMode = 'read' | 'write' | 'mixed';

export interface EntityViewConfig<T extends object> {
  /** Global default; per-field entries override. */
  mode: EntityViewMode;
  fields?: Partial<Record<keyof T & string, FieldAccess>>;
}

export function resolveFieldAccess<T extends object>(
  config: EntityViewConfig<T>,
  key: keyof T & string,
): FieldAccess {
  const override = config.fields?.[key];
  if (override) return override;
  if (config.mode === 'read') return 'read';
  if (config.mode === 'write') return 'write';
  return 'read'; // mixed default: safe read until configured
}

export function isWritable<T extends object>(
  config: EntityViewConfig<T>,
  key: keyof T & string,
): boolean {
  return resolveFieldAccess(config, key) === 'write';
}

export function isReadable<T extends object>(
  config: EntityViewConfig<T>,
  key: keyof T & string,
): boolean {
  const a = resolveFieldAccess(config, key);
  return a === 'read' || a === 'write';
}
```

## Extensión por dominio (ejemplo clients)

```ts
const clientsEditConfig: EntityViewConfig<ClientDto> = {
  mode: 'mixed',
  fields: {
    id: 'hidden',
    name: 'write',
    email: 'write',
    tenantId: 'read',
  },
};

const clientsDetailConfig: EntityViewConfig<ClientDto> = {
  mode: 'read',
};
```

- **Angular / Ionic:** `EntityFormViewBase<T>` (abstract) + subclass
  `ClientsFormView` que fija `config` y slots de UI (Native inputs).
- **React / Next / RN:** `useEntityView(config)` + presentational fields; o
  la misma config tipada compartida desde `@base/shared` / UI kit.

La vista **no** llama HTTP: el panel (F66-A1) usa el facade
([state-soc-facade.md](./state-soc-facade.md)). Validación: predicates
[ADR 0012](../adr/adr-0012-isomorphic-validation.md).

## Anti-patrones

- Copiar el form de create para edit cambiando dos líneas.
- `*ngIf="mode === 'edit'"` gigante por cada campo sin `FieldAccess`.
- Meter `ApiClient` en la clase de vista.

## Estado de implementación

Kernel (F66-A2) + rollout (F67-A3):

| Pieza | Ubicación |
|-------|-----------|
| Helpers + `EntityViewConfig` | `@base/shared` → `lib/entity-view` (`EntityFieldAccess` evita choque con RBAC `FieldAccess`) |
| Angular base | `@base/angular-ui` → `EntityFormViewBase` |
| React hook | `@base/react-shared` → `useEntityView` |

### Matriz dominio × stack (F67-A3)

| Dominio | Angular | React | Notes |
|---------|---------|-------|-------|
| clients | `ClientsFormViewComponent` + configs; form en FeatureShell create slot | `useClientsEntityView`; form en FeatureShell create slot | Piloto F66 + slot F67 |
| roles | configs + `RolesFormViewComponent`; `RoleForm` usa `isReadable`/`isWritable` | `useRolesEntityView` | F67-A3 |
| users | configs + `UsersFormViewComponent` | `useUsersEntityView` | F67-A3; wire pages → [F68-B3](../plans/rounds/plans-68-sixty-eight-round/1750000255000-f68-entity-view-deepen.md) |
| audit | — | — | Evaluar read-only en F68-B3 |

Validación sigue predicates / [ADR 0012](../adr/adr-0012-isomorphic-validation.md).
