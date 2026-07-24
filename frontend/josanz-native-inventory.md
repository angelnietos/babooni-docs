# Josanz ↔ native-ui inventory (F82-A1)

Matriz de decisión: `@josanz/angular-ui` frente a `@base/native-ui` / `@base/angular-ui` `Native*`.
**Sin moves físicos** en A1. Guía: [ui-re-export-vs-wrapper.md](../guides/ui-re-export-vs-wrapper.md).
Ronda: [F82](../plans/rounds/plans-82-eighty-two-round/README.md).

## Etiquetas

| Etiqueta | Significado |
|----------|-------------|
| **PROMOTE-TO-BASE** | Genérico reutilizable → enriquecer Lit SoT (+ adapter) |
| **WRAP** | SoT cubre el núcleo; Josanz aporta brand / defaults / template ERP |
| **RE-EXPORT** | Sin valor de marca → re-export `Native*` / base (YAGNI) |
| **JOSANZ-ONLY** | Solo producto (catalog, settings, delete host, Figma shells, …) |

## SoT actual (`@base/native-ui`)

24 CEs con adapters Angular `Native*` (ver [native-ui-adapter-matrix.md](./native-ui-adapter-matrix.md)):
button, input, select, checkbox, radio, toggle, alert, badge, chip, modal, toast,
spinner, skeleton, progress, empty-state, avatar, icon, card, divider, tabs,
segment, pagination, board (+ lane).

## Canarios WRAP (prioridad F82-C1)

Orden: **button → input → select → checkbox → spinner → badge → alert → modal**.

| Josanz | Path | Native SoT | Adapter | Decision | Gaps / notas |
|--------|------|------------|---------|----------|--------------|
| `ButtonComponent` | `actions/button/` | `base-button` | `NativeButtonComponent` | **WRAP** | Map `outline`→`ghost`/`secondary`; `xl` vía size o CSS; tema `--josanz-*` en host |
| `InputComponent` | `forms/input/` | `base-input` | `NativeInputComponent` | **WRAP** | CVA + field chrome Josanz; CE = value/error |
| `SelectComponent` | `forms/select/` | `base-select` | `NativeSelectComponent` | **WRAP** | CVA + hint/shape; API casi alineada |
| `CheckboxComponent` | `forms/checkbox/` | `base-checkbox` | `NativeCheckboxComponent` | **WRAP** | Description vía slot o promote genérico |
| `SpinnerComponent` | `feedback/spinner/` | `base-spinner` | `NativeSpinnerComponent` | **WRAP** | Label + accent color en wrapper |
| `BadgeComponent` | `data-display/badge/` | `base-badge` | `NativeBadgeComponent` | **WRAP** | Hoy Presentation*; size/soft/solid/dot — map o promote |
| `AlertComponent` | `feedback/alert/` | `base-alert` | `NativeAlertComponent` | **WRAP** | Hoy Presentation*; dismissible/neutral — map o promote |
| `ModalComponent` | `overlays/modal/` | `base-modal` | `NativeModalComponent` | **WRAP** | Sheet móvil + footer; CE `open` + slots |

## Oleada WRAP (post-canario)

| Josanz | Native | Decision | Notas |
|--------|--------|----------|-------|
| `SecondaryButtonComponent`, `CopyButtonComponent`, `FloatingActionButtonComponent` | `base-button` | **WRAP** | Componen el button WRAP |
| `SwitchComponent` | `base-toggle` | **WRAP** | |
| `RadioGroupComponent` | `base-radio` | **WRAP** | Orquestación Angular |
| `SegmentedControlComponent` | `base-segment` | **WRAP** | |
| `EmptyStateComponent` | `base-empty-state` | **WRAP** | Hoy Presentation* |
| `SkeletonComponent` | `base-skeleton` | **WRAP** | |
| `InlineAlertComponent` | `base-alert` | **WRAP** | |
| `ProgressBarComponent` | `base-progress` | **WRAP** | |
| `ToastComponent` | `base-toast` | **WRAP** | `showToast` + host |
| `CardComponent` | `base-card` | **WRAP** | Header/footer ricos |
| `TagComponent` | `base-chip` | **WRAP** / **RE-EXPORT** | Si sin marca → chip |
| `DividerComponent` | `base-divider` | **RE-EXPORT** / **WRAP** | Label ya en CE |
| `PaginationComponent` | `base-pagination` | **WRAP** | Chrome Josanz |
| `ConfirmDialogComponent` | `base-modal` | **WRAP** | Composite |
| `NumberInput*`, `Password*`, `Search*`, `Phone*`, `Currency*` | `base-input` | **WRAP** | Composites de input |
| `ChipInputComponent` | `base-chip` | **WRAP** | |
| `KanbanBoardComponent` | `base-board` | **WRAP** | Estilo producto |
| `AvatarGroupComponent` | `base-avatar` | **WRAP** | Composite |

## PROMOTE-TO-BASE (F82-B1 — solo gaps canario / reuso claro)

| Gap | Evidencia | Acción F82 |
|-----|-----------|------------|
| Alert: `dismissible`, tone `neutral` | PresentationAlert + Josanz Alert; útil Arquetipos/SaaS | Enrich `base-alert` + adapter |
| Badge: size, `soft`/`solid`, removable, dot | PresentationBadge parity | Enrich `base-badge` + adapter |
| Button: alias `outline` (o doc `ghost`), size `xl` | Josanz Button API | Enrich `base-button` |
| Checkbox: description slot | Josanz Checkbox | Enrich `base-checkbox` |
| Modal: `sheet` / width contract | Josanz Modal móvil | Enrich `base-modal` si genérico |
| `base-textarea` | Josanz Textarea; genérico | Nuevo CE + `NativeTextarea` |

**Defer** (no bloquear C1): date/time pickers, autocomplete, multi-select, slider, OTP,
file upload, drawer/tooltip/popover, accordion/stepper/tree, data-table — abrir cuando
haya consumidor fuera de Josanz.

## RE-EXPORT (YAGNI)

| Export | Notas |
|--------|-------|
| `TopbarComponent` | Sin delta de marca |
| `BaseButtonComponent`, `BaseInputComponent`, … | Aliases legacy `@base/angular-ui` (escape hatch) |
| Helpers pagination / utils | Lógica compartida |

## JOSANZ-ONLY (sample — no forzar a base)

- Catalog: `JosanzCatalogListComponent`, `StatusKanbanBoardComponent`, list-export, theme rails
- Settings: `AppSettingsPageComponent`, brand/atmosphere/list/catalog panels
- Detail/delete: `JosanzFigmaDetailShellComponent`, `JosanzDeleteConfirmHostComponent`
- Product forms: `JosanzClientRailPickerComponent`, `JosanzForm*FieldComponent`
- Feedback brand: `JosanzFigmaSuccessToastComponent`, catalog load error/skeleton
- Layout ERP: `MainListLayoutComponent`, `AdaptiveListRowsComponent`, list template rows
- `UserAvatarComponent` + role badge (sesión / Figma)

## Bootstrap prerequisite

`registerNativeUi()` **ausente** en:

- `apps/clientes/josanz/frontend/josanz/src/main.ts`
- `libs/clientes/josanz/angular-ui/.storybook/preview.ts`

F82-C1 debe registrarlo (o importar `Native*` que registre CEs) antes de smoke visual.

## Folder carry (F82-D1)

Taxonomía actual (`actions|forms|feedback|…`) → `atoms|composites|theme` según
[ui-lib-folder-layout.md](./ui-lib-folder-layout.md). Wrappers Native* → `atoms/{name}/`.
Product shells → `composites/{name}/`.
