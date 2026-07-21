# Figma ↔ `@base/native-ui` (F54-D1 / F55-D1)

**Estado:** matriz documentada. **Code Connect piloto defer → F56** (sin token
Figma MCP / Code Connect en CI). Regla: **Figma → Lit CE primero**; no generar
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

Piloto Code Connect (button/input/alert) cuando haya acceso Figma.
