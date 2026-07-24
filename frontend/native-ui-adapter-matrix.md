# Native UI adapter matrix (F79-A1)

Canonical map: Lit SoT (`@base/native-ui`) → framework adapters.
Rules: [design-system.md](./design-system.md) · [ui-strategy.md](./ui-strategy.md)
(F79 cerrada, git) · ADR [0010](../adr/adr-0010-native-ui-lit-sot.md) ·
ADR [0011](../adr/adr-0011-storybook-native-ui-first.md) ·
folder layout [ui-lib-folder-layout.md](./ui-lib-folder-layout.md).

## Classes

| Class | Meaning |
|-------|---------|
| **SoT atom** | CE in `@base/native-ui`; adapters wrap or twin — no visual fork |
| **Platform chrome** | Nav, safe-area, keyboard, tab bar, Ion overlays — not parity gaps |
| **Composite** | Feature shell, page header, table toolbar — compose SoT atoms |
| **Brand** | Product/arquetipos UI — re-export or thin wrap over base adapter |

## Contract decisions

1. New atoms land in `base-native-ui` (+ Lit story) first, then adapter wave.
2. No Next/Ionic CSS forks as the visual source for SoT atoms.
3. Storybook parity = same SoT set documented; chrome-only stories are extra.
4. CSS/`ion-*` design-atom forks → `@deprecated` + Lit alias for one round; delete in F80 if stable.
5. RN: tokens + conceptual API only — **never** import `lit` / `@base/native-ui` in RN runtime.

### Migration waves (Next / Ionic)

| Wave | Atoms |
|------|-------|
| W1 | button, input, select, icon |
| W2 | alert, badge, chip, toast, empty-state |
| W3 | card, divider, avatar, skeleton, spinner, progress |
| W4 | checkbox, radio, toggle, segment, tabs, modal, pagination |
| W5 | board (+ remaining matrix gaps) |

## SoT atoms

| Atom | Lit tag | Angular (`Native*`) | React (`Native*`) | Next (`Arq*`) | Ionic (`ArqIon*`) | RN (`Arq*`) | Notes |
|------|---------|---------------------|-------------------|---------------|-------------------|-------------|-------|
| button | `base-button` | `NativeButtonComponent` | `NativeButton` | `ArqButton` → Native | `ArqIonButton` → Lit | `ArqButton` OK | F82: variants + `outline`; sizes + `xl` |
| alert | `base-alert` | `NativeAlertComponent` | `NativeAlert` | `ArqAlert` → Native | inline SoT via CE / host; `ArqIonAlert` = **chrome** (`ion-alert`) | `ArqAlert` OK | F82: `dismissible` + tone `neutral`; Ion overlay kept for mobile a11y |
| card | `base-card` | `NativeCardComponent` | `NativeCard` | `ArqCard` → Native | `ArqIonCard` → Lit | `ArqCard` OK | |
| checkbox | `base-checkbox` | `NativeCheckboxComponent` | `NativeCheckbox` | — | — (use CE) | **gap** | F82: `description` prop |
| chip | `base-chip` | `NativeChipComponent` | `NativeChip` | — | — (use CE) | **gap** | |
| divider | `base-divider` | `NativeDividerComponent` | `NativeDivider` | — | — (use CE) | `ArqDivider` OK | |
| empty-state | `base-empty-state` | `NativeEmptyStateComponent` | `NativeEmptyState` | `ArqEmptyState` → Native | `ArqIonEmptyState` → Lit | `ArqEmptyState` OK | |
| icon | `base-icon` | `NativeIconComponent` | `NativeIcon` | `ArqIcon` → Native | CE in features | **gap** (use text/emoji) | |
| input | `base-input` | `NativeInputComponent` | `NativeInput` | `ArqInput` → Native | `ArqIonInput` → Lit | `ArqInput` OK | |
| modal | `base-modal` | `NativeModalComponent` | `NativeModal` | — | `ArqIonModal` = **chrome** | `ArqModal` OK | Prefer Ion overlay on mobile |
| pagination | `base-pagination` | `NativePaginationComponent` | `NativePagination` | — | — | **gap** / N/A mobile | |
| progress | `base-progress` | `NativeProgressComponent` | `NativeProgress` | — | CE in demos | **gap** | |
| radio | `base-radio` | `NativeRadioComponent` | `NativeRadio` | — | — | **gap** | |
| segment | `base-segment` | `NativeSegmentComponent` | `NativeSegment` | — | `ArqIonSegment` → Lit | `ArqSegment` OK | |
| select | `base-select` | `NativeSelectComponent` | `NativeSelect` | `ArqSelect` → Native | — (use CE) | `ArqSelect` OK | |
| skeleton | `base-skeleton` | `NativeSkeletonComponent` | `NativeSkeleton` | — | `ArqIonSkeleton` → Lit | `ArqSkeleton` OK | |
| spinner | `base-spinner` | `NativeSpinnerComponent` | `NativeSpinner` | `ArqLoadingState` → Native | — (use CE) | `ArqSpinner` OK | |
| tabs | `base-tabs` | `NativeTabsComponent` | `NativeTabs` | — | `ArqIonTabs` = **chrome** | `ArqTabs` OK | Ion tabs = chrome |
| toast | `base-toast` | `NativeToastComponent` | `NativeToast` | `showToast` → native | `showArqIonToast` chrome | `useArqToast` OK | |
| toggle | `base-toggle` | `NativeToggleComponent` | `NativeToggle` | — | CE in demos | `ArqToggle` OK | |

## Platform chrome / composites (not SoT parity gaps)

| Export | Package | Class |
|--------|---------|-------|
| `ArqPageHeader`, `ArqTable`, `ArqTableToolbar`, `ArqConfirmDialog`, `ArqErrorState` | `@base/next-ui` | Composite |
| `ArqIonFeatureShell`, `ArqIonPageHeader`, `ArqIonTabs`, `ArqIonSearch` | `@base/ionic-ui` | Platform chrome |
| `ArqIonAlert`, `ArqIonModal`, `showArqIonToast`, `confirmArqIonDanger` | `@base/ionic-ui` | Platform chrome (overlays) |
| `ArqMobileShell`, `ArqPageHeader`, `ArqList`, `ArqSearch`, `ArqConfirmDialog` | `@base/react-native-ui` | Platform chrome / composite |

## Status legend (RN column)

- **OK** — twin present; props/tokens aligned to Lit conceptual API
- **gap** — twin missing or incomplete; documented, not counted as Next/Ionic fork failure
- **N/A** — not applicable on mobile
- **chrome** — platform overlay/nav; intentional Ion/RN native
