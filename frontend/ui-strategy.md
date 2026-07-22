<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Estrategia UI — Lit native-ui + wrappers por framework</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

Cuándo usarla: debates “¿componentes nativos compartidos o solo Angular Material
/ shadcn por stack?”, Storybook, o al añadir un primitivo nuevo.

**ADR:** [0010](../adr/adr-0010-native-ui-lit-sot.md) (SoT + freeze) ·
[0011](../adr/adr-0011-storybook-native-ui-first.md) (Storybook).

Planes: [F53](../plans/rounds/plans-53-fifty-three-round/) ·
[F54](../plans/rounds/plans-54-fifty-four-round/) ·
[F60](../plans/rounds/plans-60-sixty-round/) (login/clients) ·
**[F62](../plans/rounds/plans-62-sixty-two-round/)** (native-first features,
temas, select).

**Arquetipos demo (F62):** el camino feliz de features es **solo** `Native*` /
`ArqNative*` sobre `@base/native-ui`. No `<select>` OS; no PageHeader/TableToolbar
framework en páginas de producto. `/native-ui` muestra el catálogo real, no un
piloto F35.

---

## 1. Problema que resolvemos

Sin estrategia común:

- Button distinto en Angular, React, Next, RN → N implementaciones.
- La IA inventa markup por feature.
- Cambiar design tokens implica chase en 4 repos mentales.

Con estrategia:

```
Una SoT de primitivos (Lit CE) → wrappers idiomáticos → marca de producto
```

---

## 2. Capas UI en este monorepo

| Capa | Paquete | Tecnología | Rol |
|------|---------|------------|-----|
| **A. Primitivos crosscutting** | `@base/native-ui` | Lit Custom Elements | **SoT** button, input, card, modal… |
| **B. Adapter framework** | `@base/angular-ui`, `@base/react-ui`, … | Wrappers `Native*` (+ legacy) | Tipado, CD, props; **capar/extender** CE |
| **B′. RN** | `@base/react-native-ui` | Primitivos RN | Misma API/tokens conceptuales; **no** Lit |
| **C. Marca / plantilla** | `@josanz/angular-ui`, `@arquetipos/*-ui`, `@saas/shared-ui` | Extiende B | Branding, defaults |

Regla wrap vs re-export en C: [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md).

---

## 3. Freeze (F53+) — obligatorio

> **A partir de ahora** no se añaden ni se evolucionan (features nuevas, variantes,
> rediseños) primitivos **solo-framework** en `@base/angular-ui` /
> `@base/react-ui` / `@base/ionic-ui` salvo bugs críticos de seguridad/regresión.

| Permitido | No permitido |
|-----------|--------------|
| Nuevo CE en `@base/native-ui` | Nuevo `ButtonComponent` Angular paralelo en base |
| Wrapper `NativeX` que envuelve el CE | Copiar el mismo átomo a React “porque el CE no tiene wrapper aún” sin abrir gap A2 |
| Wrapper de **marca** (Josanz/Arquetipos) sobre Native* o CE | Feature con HTML/CSS suelto “provisional” eterno |
| Fix bug en legacy framework-only | Rediseño visual solo en legacy framework-only |
| Alinear tokens RN ↔ native | Meter Lit en el bundle RN |

Los primitivos framework-only **ya existentes** en base se mantienen en el
código hasta migración/deprecación (F54-A1/A3); **no** son el camino de
crecimiento. Integrar UI de otros proyectos → Lit / wrappers (F53-A3, F54-A2),
nunca islas nuevas en base.

**Apps** (arquetipos, clientes, SaaS): priorizar `Native*` / `ArqNative*` /
CE en hosts Ionic.

---

## 4. Por qué Lit / Custom Elements

- Corren en **cualquier** host DOM (Angular, React, Next, Ionic, estático).
- Encapsulación Shadow DOM / estilos acotados.
- Una base común sin forzar un solo framework FE.
- RN: tokens / API conceptual — ver §3 y F54-B2 (`@base/ui-tokens`).

---

## 5. Flujo al añadir un primitivo (ej. `Badge`)

1. ¿Es genérico? → **obligatorio** `@base/native-ui` (+ tests + Storybook native).
2. Wrapper tipado en `@base/angular-ui` / `@base/react-ui` (`NativeBadge`).
3. Actualizar [ui-component-catalog.yaml](./ui-component-catalog.yaml)
   (marcar `native` vs `wrapper` vs `legacy-framework`).
4. Producto: re-export o wrapper de marca.
5. `pnpm check:ui-ownership` (+ futuro `check:ui-native-first`).

No empieces el primitivo dentro de una feature Josanz ni como Angular-only en base.

---

## 6. Storybook

| Prioridad | Proyecto | Comando (objetivo F53) |
|-----------|----------|------------------------|
| 1 — SoT | `base-native-ui` | `pnpm nx storybook base-native-ui` (puerto ~4400) |
| 2 — Adapters | `base-angular-ui`, `base-react-ui` | `nx storybook …` (~4402 / 4403) |
| 3 — Marca | `josanz-angular-ui` | ya serve :4401 |

- Documenta variantes del **paquete que posee** el componente.
- Detalle: [ADR 0011](../adr/adr-0011-storybook-native-ui-first.md),
  [design-system.md](./design-system.md).

---

## 7. Relación con “stack oficial Next de la empresa”

- Los **primitivos Lit** no acoplan el design system a Tailwind de un solo stack.
- Next envuelve CEs o usa `@base/next-ui`.
- Elección de stack de producto: [framework-decision-guide](../architecture/framework-decision-guide.md).

---

## 8. Anti-patrones

| Mal | Bien |
|-----|------|
| Copiar Button Angular a React a mano | Extender native-ui + wrapper |
| Feature con CSS suelto “provisional” eterno | Token / UI package |
| Wrapper Josanz vacío “por si acaso” | Re-export |
| Nuevo primitivo framework-only en base | CE + `Native*` |
| Feature plantilla importando `ArqButton` / `ButtonComponent` legacy | `ArqNative*` / `Native*` (`check:ui-native-first`) |
| Un Storybook de producto mezclando capas base | Ownership por paquete; SB native primero |

---

## Enlaces

- [design-system.md](./design-system.md)
- [how-it-works.md](./how-it-works.md)
- [deprecation-policy.md](../guides/deprecation-policy.md) — deprecar legacy UI
- Código: `libs/base/frontend/crosscutting/native-ui/`
