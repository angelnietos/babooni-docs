# Figma ↔ `@base/native-ui` (F54-D1)

**Estado:** matriz documentada sin Code Connect activo (sin token Figma MCP /
Code Connect en CI). Regla: **Figma → Lit CE primero**; no generar wrappers
Angular/React saltándose `@base/native-ui`.

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

Cuando haya acceso Figma: piloto Code Connect en button/input/alert y ampliar.
