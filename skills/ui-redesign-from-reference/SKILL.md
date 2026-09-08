---
name: ui-redesign-from-reference
description: Produce a comprehensive UI/UX redesign plan for an existing application by translating a visual reference (HTML/JSX prototype, screenshots, Figma URL, design bundle, mockup file, or written description) onto the production codebase while preserving 100% of current functionality. ALWAYS use this skill when the user asks to "redesign the UI", "apply this design", "match this mockup", "restyle the app", "update the theme", "rebrand the look", "refresh the visual style", "port these designs", "update the UI to match", or anything that pairs a visual reference with an existing app that needs to keep working. Use it even when the user does not say the word "plan" — the deliverable is always a written plan, not code edits. Skill produces a single Markdown document; it does not modify source code.
---

# UI Redesign From Reference

A redesign-planning workflow for projects that have shipped real functionality and now need to change *how they look* without changing *what they do*. The output is one Markdown plan document that an engineer (or another Claude session) can execute against.

The cardinal rule of this skill, repeated in every section because it is the rule users care about most:

**Theme/style only. The plan must not change any current functionality, route, form field, filter, workflow, API contract, or data flow — even when the reference design suggests otherwise.**

When the reference design and the production app disagree about *what the page does* or *what choices the user has*, the production app wins. When they disagree about *what it looks like*, the reference wins.

---

## When to use this skill

Trigger this skill when **all three** of the following are true:

1. A working production app exists in the current working directory (or the user has named one).
2. The user has provided — or is about to provide — a visual reference: an HTML/JSX bundle from a design tool, a Figma URL, a screenshot set, a Sketch/PNG/PDF mockup, a competing app to mimic, a written description of a new look, or a brand guide.
3. The user wants the app's surface to look more like the reference, with no implication of behavior changes.

If any of those is missing, do not use this skill. In particular, do not use it when the user wants you to *build a new screen* (use a frontend-design skill instead) or when there is no production app yet (no functionality to preserve).

---

## Workflow

The workflow has five phases. Do them in order. Do not start writing the plan until Phase D.

### Phase A — Gather the reference

Get whatever the user has into your context.

- **HTML/JSX prototype bundle (claude.ai/design exports, "handoff" archives):** read the entrypoint HTML in full, then follow its imports — every script tag, every linked CSS file, every JSX file pulled in. Do not skim. The structure, the inline styles, the token names, the icon set, and the animation keyframes are all in those files.
- **Figma / Sketch URL:** ask the user for an exported HTML/CSS dump or PNG screenshots. Do not try to render the URL in a browser.
- **Screenshots:** read them with the Read tool (it accepts images). Note typography, palette, spacing, radii, and component shapes. Ask the user about anything ambiguous.
- **Written description or brand guide:** ask follow-up questions until you have at minimum a palette (5–10 swatches), typography intent (display vs body vs mono), spacing rhythm, and at least one example screen sketched in words.
- **"Match this competitor's app":** ask for screenshots or a public URL of specific pages. Don't generalize.

By the end of Phase A you should be able to answer: what are the tokens (colors, fonts, radii, spacings), what are the animation idioms, what icon family is used, and what's the high-level layout shape (sidebar/tab nav/single-column/etc.).

### Phase B — Audit the current codebase

In parallel, do an inventory of the production app. **Delegate the bulk of this to subagents** — it is naturally parallel and will otherwise blow out the main context window.

For each top-level route or screen:

- Top-level layout (containers, headers, sections in order).
- Styling system in use (Tailwind classes, CSS-in-JS, shadcn, Material, Bootstrap, custom tokens like `--v2-*`).
- Interactive elements (forms, dropdowns, tables, dialogs, accordions, tabs, sheets, popovers).
- Data flow (REST, GraphQL, direct DB client, realtime subscriptions).
- Mobile vs desktop differences visible in the code (`hidden md:*`, breakpoint branches).
- Any inconsistencies or partial-migration smells (raw hex tokens, off-system colors, light-themed components in a dark app, etc.).

Also inventory:

- The shared shell / layout component(s).
- The list of installed UI primitives (shadcn `ui/`, Radix, MUI components in use).
- The CSS file(s) where global tokens live.
- Auth gating, role gating, setup wizards, onboarding overlays.
- Realtime channels and their filter shapes (so the plan can promise to keep them unchanged).

Capture this as notes in your context — you will write from it in Phase D, you don't need to file it anywhere yet.

### Phase C — Reconcile with the user; lock the constraints

This phase is where most redesigns go wrong. The reference will *always* contain things that look like UX improvements but are actually behavior changes — a simpler form, a different tab structure, a new "drawer" instead of a page, fewer toggles, a different navigation depth. **You must surface every one of these to the user before writing the plan**, and let the user decide.

For each apparent prototype-vs-production divergence, classify it using the rubric in `references/function-vs-style.md` (read that file now if you haven't). The classification is binary: every divergence is either **style** (in scope — adopt) or **function** (out of scope — preserve production).

Common divergences and the default classification:

| Reference shows… | Production has… | Default |
|---|---|---|
| Different palette / tokens / typography / icons / radii / spacing / shadows / animations | Different set | **STYLE — adopt** |
| Different sidebar style / nav bar visual treatment / card chrome | Different chrome | **STYLE — adopt** |
| Different layout switcher (e.g., Podium / List / Grid) | One fixed layout | **FUNCTION — preserve** (new affordance) |
| Different number of tabs / different tab labels in a filter | Existing filter | **FUNCTION — preserve** |
| Drawer/sheet instead of a page route (or vice versa) | Existing pattern | **FUNCTION — preserve** (deep-linkability changes) |
| Toggleable form vs always-visible form | Existing pattern | **FUNCTION — preserve** |
| Simpler form with fewer fields | Full form | **FUNCTION — preserve** (data loss risk) |
| New info surface (e.g., "Live Activity" feed, "Your Week" sparkline, recommendation card) | No such surface | **FUNCTION — preserve** (cut it) |
| Theme switcher (light + dark) | Single theme | **STYLE — but ask** (cheap to add; the user may not want the scope) |
| Different routes / new top-level routes | Existing routes | **FUNCTION — preserve** |
| Different role gating, different mode toggles | Existing gating | **FUNCTION — preserve** |
| Different default values, fewer or more filter options | Existing defaults/options | **FUNCTION — preserve** |

After the classification pass, **present the user with the list of FUNCTION-class divergences** and ask which they want to adopt anyway, which to drop, and which to defer. Use the `AskUserQuestion` tool when 1–4 binary or small-multiple decisions are needed. Use plain prose for longer judgment calls.

Examples of questions to ask, depending on the project:

- "The reference cuts the Monthly/Quarterly toggle. Production has it. **Keep it?**" → expect "yes, keep it"
- "The reference has a Whiteboard grid view that doesn't exist today. **Add it (new feature) or skip (out of scope)?**"
- "The reference defines light + dark themes. **In scope for this redesign or dark-only?**"
- "The reference simplifies the admin scoring form to a tech-picker + 5-button points control. Production has 8 fields. **Keep the full production form?**" → expect "yes"

**Do not start Phase D until the user has explicitly resolved these.** If the user says "you decide," default to the production-preserving choice for every FUNCTION-class divergence.

### Phase D — Compose the plan document

Write a single Markdown file in the *project root* (not in `src/`, not in the worktree's `.claude/`). Name it `UI_Redesign_Plan_<short-name>.md` where `<short-name>` is the project / theme name in PascalCase.

Use the template in `references/plan-template.md` as the skeleton — it has the exact section structure and ordering that works. Adapt section content to the project, but keep the structure. The structure is non-negotiable for two reasons: (1) it puts the "no functional changes" guarantees in front of every other discussion, and (2) it gives the team a single document they can use as a contract.

The minimum sections, in order, are:

1. **Header block** (version, date, author, sources)
2. **§0 Operating principle** — the theme/style-only rule, restated explicitly with the user's resolved decisions. Include a "Prototype shows X / Production has Y / v3 does Z" table for the FUNCTION-class divergences.
3. **§1 Executive summary** — what changes, what doesn't, sized roughly in days/phases.
4. **§2 Naming convention** — how the old and new token / utility names coexist during the migration (e.g., `--v2-*` parallel to `--gx-*` until cleanup phase). This prevents grep collisions.
5. **§3 Design system delta** — the authoritative crosswalk:
   - Token map (old → new) for every color, surface, border, text level
   - Typography (body / display / mono families and weights)
   - Spacing & radius reference table
   - Animation keyframes (declared once in the global stylesheet)
   - Component primitives (the small library of new presentation components — Icon, Avatar, Score, Pill, Card, etc.)
6. **§4 Navigation & shell** — what the desktop and mobile shells look like before and after, item by item. Be explicit about anything that *stays the same* (this is reassuring and prevents creep).
7. **§5 Screen-by-screen plan** — one subsection per route. Each has two blocks:
   - **Behavior preserved:** numbered list of what must keep working exactly.
   - **Visual changes:** numbered list of strictly theme/style updates.
   - If a screen is currently off-system (e.g., light-themed in an otherwise dark app), call it out explicitly as a **regression to fix**.
8. **§6 Inconsistency inventory** — every gap between the current UI and the target. Tag each with P0/P1/P2 severity. Split into cross-cutting and per-screen.
9. **§7 Build phases** — typically 4–6 phases. Phase 0 = approval; Phase 1 = design system + primitives; Phases 2–4 = screen reskins grouped by area; final phase = cleanup, motion gating, full regression sweep. Each phase ends in a shippable state with explicit exit criteria.
10. **§8 File-by-file impact list** — every file that changes, what changes (e.g., "visual only", "restyle + repaint", "full rewrite"), and the phase. Also list files that are intentionally untouched (this is the "no API changes, no schema changes" guarantee made concrete).
11. **§9 Functionality preservation guarantees** — the QA rubric. Every behavior that must remain true, organized by feature area (Auth, Dashboard, Audit, Claims, Admin, etc.). This is what gets exercised in the final-phase regression sweep.
12. **§10 Risk register** — table of risks with likelihood × impact × mitigation. Especially flag risks around renames (broken imports), token migrations (grep collisions), and rewrites of pages that have realtime / state subscriptions.
13. **§11 Success criteria** — concrete pass/fail gates. Always include: (a) functional parity per §9, (b) visual parity vs the reference, (c) no automated regressions, (d) `git grep` checks for dead tokens, (e) reduced-motion respected if animations were introduced.
14. **§12 Resolved decisions log** — every Phase C decision, written as struck-through-then-resolved entries: "~~Light theme?~~ → **Out of scope. Dark only.**" This prevents the same questions getting re-litigated later.
15. **§13 Appendix — reference-to-production component map** — table mapping each named component / screen / atom in the reference to its home in the production codebase, with "not adopted" entries for anything cut.

### Phase E — Hand off

Do not modify source code. Surface the path to the plan file in your closing message and summarize the highlights — operating principle, key carve-outs from the reference, phase count, days estimate, biggest risks. Suggest the user commission Phase 1 as the next step.

---

## Patterns and pitfalls

These come from real redesigns where the plan went sideways. Read them once.

**Pitfall: adopting "obvious improvements" from the reference.** The user almost always says "preserve functionality" up front, but the reference makes simplifications that *feel* like improvements (fewer fields, fewer tabs, no toggle). Resist. The user wrote the existing app the way they did for a reason — usually a real user need, a regulatory requirement, or a hard-won lesson. The rubric in `references/function-vs-style.md` exists to give you a stable line to draw.

**Pitfall: silently adopting a different shell structure.** The reference often shows a 3-pane desktop or a different navigation depth than production. Adopting this is a *layout/navigation* change, not a *style* change. Surface it as a Phase C decision; do not slip it into the plan unannounced.

**Pitfall: introducing new info surfaces.** "Live activity feed", "your week sparkline", "what's new", recommendations — these look like the same data shown differently, but they're new product surfaces. Cut them by default; surface them as Phase C decisions if they appear in the reference.

**Pitfall: writing the plan before Phase C.** The temptation is high to draft a 6,000-word plan immediately. Don't. Walk the user through the FUNCTION-class divergences first, lock the decisions, *then* write. A revised plan with retracted decisions is a worse artifact than a plan that was right the first time.

**Pitfall: renaming production components in the plan.** Renames break imports and are a source of avoidable risk. Rewrite component *bodies* in place; reserve renames for cases where the new name is clearly more accurate (e.g., `PrizePotBar` → `PrizePotHero` when the bar genuinely becomes a hero). Document every rename explicitly in §8.

**Pitfall: missing the regression-class screens.** If the production app shows signs of a partial-migration ("v2" tokens alongside raw hex, some screens in dark theme and others in light), the new redesign should *complete the migration*. Call out the off-system screens in §5 as "currently light-themed — full dark rewrite" so the reviewer knows they're not just restyle-and-go.

**Pitfall: skipping the parallel subagent inventory.** If you read every file inline, you will blow out the context window before Phase D and the plan will be thin or contradictory. Delegate the audit. Pass the agent a precise file list and ask for ~6–10 lines per file. See `references/audit-prompt-template.md` for the prompt shape.

---

## What the deliverable looks like

A single Markdown file (typically 800–2,500 lines depending on app size) at the project root. Sections per §Phase D. Tables for token maps and file-by-file impact. Sub-bullets for behavior-preserved blocks. No code blocks beyond CSS variable / token declarations and the occasional 5–10 line snippet illustrating a token treatment.

The plan is **for humans first, executable second**. An engineer should be able to read §0 + §3 + §5 + §7 in 20 minutes and understand the scope. An AI agent should be able to read the whole thing and execute Phase 1 without further clarification.

Never write the plan into `src/`. Never write it into the worktree's hidden `.claude/` folder. Put it at the project root where it sits next to `README.md` and any prior plan artifacts.

---

## Bundled references

- [function-vs-style.md](references/function-vs-style.md) — Rubric for classifying every prototype-vs-production divergence as STYLE (adopt) or FUNCTION (preserve). Read this before Phase C.
- [plan-template.md](references/plan-template.md) — Section-by-section skeleton for the redesign plan document. Use this as the starting structure in Phase D.
- [audit-prompt-template.md](references/audit-prompt-template.md) — Prompt shape for delegating the production-code inventory to a parallel subagent in Phase B.
