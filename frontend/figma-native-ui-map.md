<p align="center">
  <img src="../assets/arquetipos-mark.svg" width="56" alt="Arquetipos" />
</p>

<h1 align="center">Figma ↔ `@base/native-ui` (F54-D1 / F55-D1 / F58-B2)</h1>

<p align="center">
  <img alt="frontend" src="https://img.shields.io/badge/frontend-0f766e?style=flat-square" />
  <a href="../README.md"><img alt="Biblia" src="https://img.shields.io/badge/hub-biblia-0f766e?style=flat-square" /></a>
</p>

**Estado:** matriz documentada. **Code Connect piloto defer → F59** (sin acceso
Figma / Code Connect en CI). Regla: **Figma → Lit CE primero**; no generar
wrappers Angular/React saltándose `@base/native-ui`.

| Figma (nombre sugerido) | CE tag | Wrapper Angular | Wrapper React |
|-------------------------|--------|-----------------|---------------|
| Button / Primary | `base-button` | `NativeButtonComponent` | `NativeButton` |
| Input / Text | `base-input` | `NativeInputComponent` | `NativeInput` |
| Alert / Banner | `base-alert` | `NativeAlertComponent` | `NativeAlert` |
| Badge | `base-badge` | `NativeBadgeComponent` | `NativeBadge` |
| Card | `base-card` | `NativeCardComponent` | `NativeCard` |
| Modal / Dialog | `base-modal` | `NativeModalComponent` | `NativeModal` |
| Tabs | `base-tabs` | `NativeTabsComponent` | `NativeTabs` |
| Toast | `base-toast` | `showToast` / host | idem |
| Divider | `base-divider` | `NativeDividerComponent` | `NativeDivider` |
| Progress | `base-progress` | `NativeProgressComponent` | `NativeProgress` |
| Chip | `base-chip` | `NativeChipComponent` | `NativeChip` |
| Select | `base-select` | `NativeSelectComponent` | `NativeSelect` |
| Icon | `base-icon` | `NativeIconComponent` | `NativeIcon` |
| Pagination | `base-pagination` | `NativePaginationComponent` | `NativePagination` |

Piloto Code Connect (button/input/alert + `*.figma.ts`) → **F59** cuando haya
acceso Figma.
