# Function vs Style — classification rubric

Use this in Phase C when reconciling the reference design with the production app. Every divergence between the two is either:

- **STYLE** — Adopt the reference. It's a surface-level change.
- **FUNCTION** — Preserve the production. It's a behavior change.

The default for borderline cases is **FUNCTION (preserve)**. A redesign that leaves a feature intact is recoverable; a redesign that quietly removes a feature is a bug report waiting to happen.

---

## STYLE — adopt from the reference

These do not change what the app does or what the user can do. Adopt them freely.

| Category | Examples |
|---|---|
| **Colors / tokens** | Background, surface, border, text-primary, text-secondary, text-muted, semantic accents (success/warning/error/info), tinted variants for backgrounds |
| **Typography** | Body family, display family, monospace family, weights, letter-spacing, line-height |
| **Type scale** | Specific font sizes for H1/H2/eyebrow/body/small/mono |
| **Spacing rhythm** | Padding inside cards, gaps between sections, vertical spacing between rows |
| **Radii** | Card corners, pill corners, input corners, avatar shape |
| **Shadows / elevation** | Depth treatment, glass / blur effects |
| **Iconography** | Icon family (Lucide vs custom SVG set vs Heroicons), stroke weight |
| **Animations** | Shimmer, pulse, fade, slide-in/-up, count-up — when they decorate existing surfaces |
| **Chrome of existing components** | Card backgrounds, hover states, active-state treatments, focus rings, tab indicator styles |
| **Sidebar / nav visual treatment** | Active-item indicator, item paddings, logo style — *as long as the item list and the destinations are unchanged* |
| **Mobile / desktop responsive thresholds** | The breakpoint at which a layout reflows — only when the existing reflow points are wrong |
| **Empty states, loading skeletons, toasts** | The look of these surfaces |

---

## FUNCTION — preserve the production

These change what the user can do, what the system stores, or what surfaces exist. Default to preserving production. Ask the user before adopting.

| Category | Examples |
|---|---|
| **Routes / URLs** | Adding `/resources` when the existing app uses a `<Sheet>` menu, or removing a route entirely |
| **Form fields** | Cutting fields the production form has (address, reasoning, supporting content). Adding new required fields |
| **Form layouts that change interaction** | Replacing a free-form points input with a fixed [+1/+3/+5] segmented control |
| **Filters / tab structures** | Reducing a Pending/Approved/Denied filter to one feed. Reducing tabs |
| **Toggles / switches that affect mode** | Adding a Technician/Admin mode toggle. Removing a Monthly/Quarterly view selector |
| **Default values** | Different default page on login. Different default filter |
| **Navigation depth** | Drawer/sheet replacing a deep-linked page (or vice versa) |
| **New info surfaces** | "Live activity" feeds, recommendation cards, "your week" stats, anything that introduces a new data display |
| **Realtime channels / data subscriptions** | Adding a new subscription. Changing channel filter shape |
| **Role gating** | Showing admin-only surfaces to non-admins, or vice versa |
| **Auth / setup gating** | Changing what happens before setup is complete, or before onboarding is complete |
| **Workflow steps** | Adding or removing a step in a wizard. Changing the gating between steps |
| **Confirmation dialogs** | Removing a destructive-action confirmation. Adding one where speed matters |
| **API contracts** | New endpoints, new request fields, new response fields |
| **Schema / migrations** | New columns, new tables, dropped columns |
| **Voting / scoring / business logic** | How a score is computed. How a vote is recorded. How a payout is calculated |
| **Privacy / visibility boundaries** | Who can see whose data — peer visibility on a leaderboard vs personal-only on history |

---

## Borderline cases and how to call them

Some changes are genuinely borderline. For these, ask the user, but here are sensible defaults:

**Light theme support when production is dark-only.** The reference defines both themes; the production is single-theme. Technically a STYLE change (just more tokens), but the rollout cost is non-zero (every screen must work in both themes, contrast bugs are real, the user has to maintain it). **Default: surface as a Phase C decision; recommend "ship single-theme only" unless the user has a stated multi-theme need.**

**Component renames where the new name is genuinely more accurate.** Renaming `PrizePotBar` to `PrizePotHero` when the new design genuinely makes it a hero element — useful for code clarity, low risk if there's one consumer. **Default: rename it if there are <5 consumers; document the rename in the impact list.**

**Replacing a Lucide icon with a custom SVG icon of the same concept.** STYLE — pure visual swap, same affordance. **Default: adopt.**

**Replacing shadcn `<Tabs>` with a custom segmented control.** Looks STYLE but it isn't — shadcn `<Tabs>` carries built-in keyboard navigation, ARIA roles, focus management. A custom replacement can regress accessibility. **Default: preserve shadcn `<Tabs>` for screens that must remain accessible; allow the custom control for cosmetic non-tab segmented selectors (e.g., a layout switcher that isn't doing semantic tab-panel switching).**

**Right-rail or third pane on desktop.** The reference shows a layout container that the production app doesn't have. The container itself is STYLE if it holds existing surfaces; it's FUNCTION if it introduces new ones. **Default: don't add the rail unless its content is strictly relocated existing surfaces.**

**Drawer overlay in addition to (not instead of) the existing page route.** Additive — preserves the deep-link and adds a quick-look overlay. STYLE-leaning but it's a new affordance. **Default: skip unless the user specifically asks. Adding optionality grows the QA surface.**

**Mode toggle that exists in production but is non-functional.** If the production app has a UI affordance that does nothing (dead button, dead toggle), removing it is a STYLE-class cleanup that happens to be functional. **Default: remove it and call it out as a side benefit.**

**Renaming a sidebar item from "My audit" to "Audit" (or similar copy tweaks).** Copy changes are FUNCTION-class because they affect navigation. **Default: preserve production copy unless the user specifically asks; flag inconsistencies as a P2 cleanup item.**

---

## How to phrase the question to the user

Use one-sentence questions that name the divergence and the default:

- > "The reference cuts the Monthly/Quarterly toggle on the dashboard. Production has it. **Keep the toggle?**" — expect: yes
- > "The reference shows a 'Live Activity' feed in a right rail. Production has no such surface. **Add it?**" — expect: no
- > "The reference uses a drawer for event detail instead of the `/event/[id]` page. **Keep the page, add the drawer, or replace the page with the drawer?**" — usually: keep the page

Don't bury these in prose. Use `AskUserQuestion` when you have 1–4 binary or small-multiple decisions; use a numbered list otherwise.

---

## When the user overrides the rubric

The user can always say "yes, adopt the simpler form, the production form has too many fields anyway." That's fine — it's their app. Capture the override in the plan's **§12 Resolved decisions log** with the strike-through-then-resolved format:

> ~~Adopt the simplified admin scoring form (tech pills + points control)~~ → **User confirmed: cut the existing visibility radio + service address + reasoning fields; keep only technician, category, action, date, description. Notification handling unchanged.**

This way the plan is auditable: a reader can see what was preserved by default and what the user explicitly chose to change.
