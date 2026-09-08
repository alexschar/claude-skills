# UI Redesign Plan — section template

The skeleton below is the structure the plan document must follow. Section ordering is non-negotiable; the carve-out tables in §0 come before *anything else* so a reviewer sees the operating principle before any aesthetic discussion.

Replace bracketed placeholders. Add or remove sub-bullets as needed for the project, but don't reorder sections, don't merge sections, and don't drop sections without an explicit reason.

---

```markdown
# [App Name] — UI Redesign Plan v[N]

**Document version:** [1.0 | 2.0 (revised per user constraints)]
**Prepared:** [YYYY-MM-DD]
**Author:** [Lead UX/UI engineer | name]
**Status:** Proposed — awaiting approval
**Sources:**
- New design bundle: [absolute path to reference files]
- Production app: [absolute path to source]
- Prior redesign artifacts (if any): [paths]

---

## 0. Operating principle — read this first

**This is a theme/style redesign only. It does not change any current functionality.**

The reference at [path/url] is the **visual reference**. We adopt its [palette, typography, spacing, icon set, component idioms, animation language]. We do not adopt anything from the reference that would change behavior, workflow, information architecture, navigation routes, form fields, filters, or data surfaces in the production app.

When the reference and the production app disagree about *what the page does* or *what choices the user has*, the production app wins. When they disagree about *what it looks like*, the reference wins.

[Explicit examples table — fill from Phase C decisions:]

| Reference shows… | Production has… | What v[N] does |
|---|---|---|
| [divergence A] | [production behavior] | **Keep [production behavior].** Restyle only. |
| [divergence B] | [production behavior] | **Out of scope.** [reason] |

Off-limits (no changes whatsoever):

- All `[api directory]` routes
- [Database/schema, RLS, migrations if applicable]
- [Business logic library paths]
- [Realtime channels, filter shapes]
- [Auth gating, setup wizard flow]
- [Onboarding overlay slide content, role gating]
- Business rules in [link to CLAUDE.md or equivalent doc]

---

## 1. Executive summary

[2–4 paragraphs covering: state of the current UI, what the proposed redesign changes, what it preserves, rough sizing in days/phases.]

What does not change:
- [bullet — concrete user-facing behaviors]
- [bullet — concrete data flows]
- [bullet — concrete layout containers]

What does change:
- [bullet — token/typography/icon families]
- [bullet — specific component visual upgrades]
- [bullet — explicit regressions being fixed (e.g., light-themed component in a dark app)]

The work is sized at approximately [N] engineering days across [M] phases. Every phase ends in a shippable state.

---

## 2. Naming convention

[Describe how old and new token/utility names coexist during the migration. The pattern: keep the v(N-1) tokens in place during the migration, delete them in the final phase.]

| Layer | [Old] | [New] |
|---|---|---|
| Token prefix | `--v2-*` | `--gx-*` |
| Utility prefix | `.v2-*` | `.gx-*` |
| Tailwind aliases | [...] | [...] |
| Tagged components | [names that keep their names] | same names, restyled bodies |

Components are **not** renamed. We rewrite component bodies without orphaning callers.

---

## 3. Design system delta

### 3.1 Token map

[Authoritative crosswalk. Every old token must end up in the right column.]

| Role | [Old] | [New] |
|---|---|---|
| Page background | [#…] | [#…] |
| Surface 1 (card) | [...] | [...] |
| Surface 2 (sunken / input) | [...] | [...] |
| Surface 3 (hi-elevation hover / active row) | [...] | [...] |
| Border | [...] | [...] |
| Text primary | [...] | [...] |
| Text secondary | [...] | [...] |
| Text muted | [...] | [...] |
| Reward / success | [...] | [...] |
| Reward bg tint | [...] | [...] |
| Error / deduct | [...] | [...] |
| Error bg tint | [...] | [...] |
| Accent / brand | [...] | [...] |
| Warning / queue | [...] | [...] |
| Info | [...] | [...] |
| Header treatment | [...] | [...] |
| Card radius | [...] | [...] |
| Active item indicator | [...] | [...] |

### 3.2 Typography

| Role | [Old] | [New] |
|---|---|---|
| Body / UI | [...] | [Inter / SF Pro / system] |
| Display headings | [...] | [Space Grotesk / Manrope / serif] |
| Tabular numerals | [...] | [JetBrains Mono / IBM Plex Mono with `tabular-nums`] |

Implementation: load via [next/font/google | @font-face | system stack]. Expose as CSS variables [--font-body, --font-display, --font-mono].

### 3.3 Animations

Declared once in [globals.css | styles/animations.css]. Pure CSS keyframes — no new motion library dependency.

```css
@keyframes [name] { ... }
```

Used by [list the consumers]. All gated behind `@media (prefers-reduced-motion: reduce)` in the final phase.

### 3.4 Component primitives

[List the new presentation primitives. Keep this small. They are presentation-only; they never own data fetching or business logic.]

| Primitive | Purpose |
|---|---|
| `<Icon name size color />` | [description, sourced from where] |
| `<Avatar />` | [description] |
| `<Score />` | [description] |
| `<DeltaPill />` | [description] |
| ... | ... |

### 3.5 Spacing & radius reference

| Token | [Old] | [New] |
|---|---|---|
| Card radius | [...] | [...] |
| Inner padding | [...] | [...] |
| Sidebar width | [...] | [...] |
| Bottom-nav height | [...] | [...] |

### 3.6 Theme switching

[Either: "Out of scope. [Theme] only." OR: a description of the data-theme attribute mechanism, cookie persistence, and the toggle component location.]

---

## 4. Navigation & shell

### 4.1 Desktop shell

[Diff table comparing today vs target for: top bar, sidebar width, sidebar logo, sidebar nav items, sidebar footer, notification bell location, theme toggle location. Be explicit about anything that stays the same.]

| Element | Today | [New] |
|---|---|---|
| ... | ... | ... |

### 4.2 Mobile shell

[Same shape as 4.1 for: top brand bar, mode toggles, bottom nav, "more" sheet/route, notification bell.]

### 4.3 Page-title responsibility

[Who owns the page title — the shell (via a switch on pathname) or each page? Default: don't move this — too risky.]

---

## 5. Screen-by-screen plan

[One subsection per route. Order matters: start with the most-trafficked screen (often `/dashboard` or `/`), then the technician-facing screens, then admin-facing screens, then auth + setup. Use this exact two-block structure for each:]

### 5.[N] `/[route]` — `[Component].tsx`

**Behavior preserved:**
- [behavior 1 — concrete, falsifiable]
- [behavior 2]
- [behavior 3]
- [realtime channel + filter shape, if applicable]

**Visual changes:**
- [change 1 — strictly theme/style]
- [change 2]
- [change 3]

**Out of scope (explicitly):** [list anything the reference shows that we are NOT adopting for this screen, with the one-line reason]

---

## 6. Inconsistency inventory

[Every gap between the current production UI and the target design. Tagged P0/P1/P2 by user-impact severity. Split into cross-cutting and per-screen.]

### 6.1 Cross-cutting

| # | Gap | Severity |
|---|---|---|
| 1 | [gap] | P0/P1/P2 |
| ... | ... | ... |

### 6.2 Per-screen

**[Screen name]** — P0/P1/P2
- [gap 1]
- [gap 2]

---

## 7. Build phases

[Sequence the work so every phase is shippable. Typical shape:]

### Phase 0 — Approval & alignment (½ day, no code)

- Confirm the plan with stakeholders.
- [Any remaining open items]

**Exit criteria:** plan accepted.

### Phase 1 — Design system & primitives ([N] days)

Foundation only. **No screens change yet.**

1. [Layout / root font setup]
2. [globals.css: token block, animations, utilities — keep old tokens parallel]
3. [Build primitives under src/components/[prefix]/]
4. [Restyle wrappers around shared chrome (toasts, etc.)]

**Exit criteria:** primitives render correctly in a sandbox; all existing screens unchanged.

### Phase 2 — [Shell + hero surfaces] ([N] days)

1. [App shell repaint]
2. [Hero / landing screen repaint with new primitives]
3. [Components that the hero depends on]

**Exit criteria:** [user-facing assertion]

### Phase 3 — [Content surfaces] ([N] days)

[List of screens in this batch]

**Exit criteria:** [user-facing assertion]

### Phase 4 — [Admin surfaces + auth + setup] ([N] days)

[List of screens in this batch + any full-rewrite regressions]

**Exit criteria:** [user-facing assertion]

### Phase 5 — Cleanup, motion gating, polish ([1] day)

1. Delete old token block + old utility classes.
2. `git grep '[old prefix]'` → zero results.
3. `git grep -E 'bg-\[#[0-9A-Fa-f]+\]'` → zero results outside intentional gradient definitions.
4. Add `@media (prefers-reduced-motion: reduce)` overrides.
5. Mobile keyboard QA: bottom nav doesn't cover focused inputs.
6. Functional regression sweep — full manual pass through [list every flow in §9].
7. Update CLAUDE.md / project README to reflect new file layout.

**Exit criteria:** every check in §9 passes; no old tokens remain; reduced-motion respected.

---

## 8. File-by-file impact list

| File | Change | Phase |
|---|---|---|
| `[path]` | [visual only / restyle + repaint / full rewrite / no change] | [N] |
| ... | ... | ... |

**Untouched (intentional):**
- All `src/app/api/**` routes
- [Other directories that must not be touched]

---

## 9. Functionality preservation guarantees

This section is the QA rubric. Every item must remain true after each phase.

**Authentication & gating:**
- [bullet]

**[Feature area 1]:**
- [bullet]

**[Feature area 2]:**
- [bullet]

[Continue for every feature area.]

---

## 10. Risk register

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| [risk] | L/M/H | L/M/H | [mitigation] |
| ... | ... | ... | ... |

---

## 11. Success criteria

The redesign is successful when:

1. **Functional parity:** every check in §9 still passes. No API changes, no schema changes, no auth changes.
2. **Visual parity:** a screenshot of every route, rendered side-by-side against the reference, matches within reasonable typographic/spacing tolerance.
3. **No regressions:** all existing automated tests pass; manual QA checklist in §7 Phase [final] passes for every role.
4. **No dead tokens:** `git grep '[old prefix]'` returns nothing.
5. **No raw hex in product chrome:** `git grep -E 'bg-\[#[0-9A-Fa-f]+\]'` returns only intentional gradient definitions.
6. **Reduced-motion respected.**

---

## 12. Resolved decisions

[Strike-through-then-resolved format. This is the audit trail.]

1. ~~[original prototype suggestion]~~ → **[user decision]**
2. ~~[original prototype suggestion]~~ → **[user decision]**
...

---

## 13. Appendix — reference-to-production component map

| Reference | Production target |
|---|---|
| `[ReferenceComponent]` | `[ProductionComponent]` |
| `[ReferenceComponent]` | **out of scope** |
| ... | ... |

---

**End of plan.**
```

---

## Sizing guidance

For a medium app (20–40 routes, partially-migrated styling, both technician and admin views), expect:

- Plan length: 1,200–2,500 lines of Markdown
- Engineering estimate: 8–15 days across 5 phases
- §5 will be the longest section by far (often half the document)
- §8 should be exhaustive — every changed file listed, every untouched directory listed
- §9 should be exhaustive — every behavior that must continue to work

For a small app (< 10 routes), the plan may be only 500–800 lines and the work 3–5 days.

For a large app (50+ routes), consider splitting the plan: the master document covers §0–§4 + §7 + §11, and a separate `Screen_Inventory.md` holds §5–§6 + §8.
