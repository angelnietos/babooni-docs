# Tenant themes checklist (F62-A1)

Cascade: `html[data-arq-tenant]` → CSS vars in `@base/ui-styles` `themes/_tenants.scss`
(+ mirrored arquetipos-theme / app styles) → Native CE + BEM pages + shell.

| Tenant | Brand | CTA | Nav active | Hero |
|--------|-------|-----|------------|------|
| **nova** | emerald `#059669` | same | mint soft | deep emerald → mint |
| **helix** | cyan `#0e7490` | coral `#f97316` | cyan soft | graphite → cyan |
| **orbit** | copper `#c2410c` | same | peach soft | charcoal → orange |

## Verify by eye (multi apps)

1. Open angular-multi / react-multi.
2. Switch tenant in topbar (listbox — not OS `<select>`).
3. Confirm **without reload**:
   - primary button / CTA color
   - sidebar active item tint + left bar
   - login hero gradient (logout → login)
4. Open `/native-ui` — primary button follows tenant.

RN: mirror brand/CTA/hero via `@base/ui-tokens/rn` + `apps/.../react-native-multi/src/tenants.ts` (token level only).
