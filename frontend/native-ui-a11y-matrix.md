# Native-ui a11y matrix (F54-B3 / F55-B3)

| CE | Role / labeling | Keyboard / focus | Status |
|----|-----------------|------------------|--------|
| `base-button` | native `<button>` | focus-visible ring | **pass** |
| `base-input` | label `for` + `aria-invalid` | tab to input | **pass** |
| `base-alert` | `role` via host content | n/a | pass |
| `base-modal` | `role=dialog` `aria-modal` | Esc/backdrop → `base-close`; Tab trap + restore focus | **pass** |
| `base-toggle` | checkbox input | space/click | pass |
| `base-tabs` | tablist / tabs + roving `tabindex` | ArrowLeft/Right, Home/End → `base-tab-change` | **pass** |
| `base-segment` | tablist / tabs + roving `tabindex` | ArrowLeft/Right, Home/End → `base-segment-change` | **pass** |
| `base-toast` | `role=status` | n/a | pass |
| `base-spinner` | sr-only label | n/a | pass |
| `base-badge` / `base-chip` | text | removable chip focus | pass |
| `base-progress` | `role=progressbar` | n/a | pass |
| `base-divider` | `role=separator` | n/a | pass |
| `base-avatar` | `role=img` + aria-label | n/a | pass |
| `base-skeleton` | `aria-hidden` | n/a | pass |
| `base-empty-state` | `role=status` | n/a | pass |
| `base-card` | presentational | n/a | pass |

Automation: `pnpm nx test base-native-ui` → `native-ui-a11y.spec.ts`.

Known gaps (F56): contrast audit with design tokens; optional live-region polish for toast stacks.
