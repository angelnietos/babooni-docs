<p align="center">
  <img src="../../../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">F66-D1 — Facade / state parity for all domains</h1>

<p align="center">
  <img alt="arquetipos" src="https://img.shields.io/badge/arquetipos-0f766e?style=flat-square" />
  <a href="./README.md"><img alt="F66" src="https://img.shields.io/badge/round-F66-14b8a6?style=flat-square" /></a>
</p>

## Estado

completado

## Objetivo

Extender el piloto F65-D1 (**clients**) a **todos** los dominios de list CRUD
arquetipos (users, roles, audit, settings donde aplique, …) con el **mismo**
contrato de puerto/facade — sin implementaciones distintas por dominio ni
“DEMO_* mutable array en la page” en unos y NgRx/RTK en otros.

**Regla:** un dominio = un facade/puerto canónico por stack; features solo
consumen ese puerto. Misma forma de API (`items` / `loading` / `error` +
`load` / `create` / `update` / `remove` según el dominio).

## Por qué

Hoy clients ya usa `ClientsFacade` / `useClientsFacade` / `IonicClientsFacade`.
Roles/users Ionic aún mezclan `DEMO_*` + helpers sueltos; Angular/React users/
roles/audit siguen patrones dual store o servicios ad hoc. Eso rompe SoC y
hace costoso swap de herramienta / tests.

## Entregables

### D1.1 — Matriz estándar (por stack)

| Stack | Herramienta detrás del puerto | Nombre API |
|-------|-------------------------------|------------|
| Angular | `EntityListStore` / service | `{Domain}Facade` (= service) |
| React | react-query (o el canónico documentado) | `use{Domain}Facade` |
| Ionic | signals facade in-memory o HTTP | `Ionic{Domain}Facade` |
| Next / RN | hook owns list | `use{Domain}Facade` |

Documentar en [state-soc-facade.md](../../../frontend/state-soc-facade.md) —
sección “matriz multi-dominio” (no solo clients).

### D1.2 — Migración por dominio (orden sugerido)

1. **users** (misma superficie CRUD que clients)
2. **roles** (create/update/remove + permisos; facade owns stub/HTTP)
3. **audit** (read-heavy: `load` + filters; sin create si no aplica)
4. **settings** solo si hay list state; si es panel de config, facade de
   settings documentado (no forzar EntityList)

Cada dominio: eliminar exports de store list CRUD deprecados; apps dejan de
registrar slices que ya no usa el panel.

### D1.3 — Uniformidad (anti-drift)

- Mismos nombres de métodos/señales entre dominios (salvo audit read-only).
- Features **nunca** importan `*Store` / `useAppDispatch` / `DEMO_*` para list
  CRUD (refuerza B3).
- Tests: mock del facade por dominio (≥1 test por dominio migrado).

## Criterios de aceptación

- [ ] Guía actualizada: matriz clients **y** users/roles/audit (+ settings si
      aplica).
- [ ] users + roles + audit con facade en Angular **y** React (mínimo); Ionic
      users + roles con facade (no DEMO en page).
- [ ] Ningún dominio list CRUD arquetipos documentado como “excepción eterna”
      sin motivo + owner.
- [ ] Typecheck + tests mock facade verdes en dominios migrados.

## Verificación

```bash
pnpm nx typecheck base-angular-users-features
pnpm nx typecheck base-angular-roles-features
pnpm nx typecheck base-react-users-features
pnpm nx typecheck base-ionic-users-features
pnpm nx typecheck base-ionic-roles-features
pnpm nx test base-react-users-features
pnpm nx test base-ionic-roles-features
```

## Depende de

F65-D1 (clients). F66-A1 (pages thin). F66-B3 (ratchet).

## Fuera de alcance

- Auth session store (sigue auth data-access).
- Josanz/SaaS product facades en esta tarjeta (pueden seguir el mismo contrato
  después).
