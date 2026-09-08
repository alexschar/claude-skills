# Audit prompt template — delegating the production inventory

In Phase B, you inventory the production codebase. For anything larger than ~10 routes, **delegate this to a parallel subagent**. Reading every file inline blows out the context window and the resulting plan will be thin.

Use the `Agent` tool with `subagent_type: "general-purpose"` (or `Explore` if available and the audit is fast/targeted). Send one prompt with a precise file list. Ask for terse output. Don't ask for code edits.

---

## Prompt shape

```
I'm doing a UI redesign audit for [App Name] ([brief stack: Next.js + Tailwind + shadcn + Supabase, dark theme, etc.]) and need a structured inventory of what each current page renders.

For each file below, give me ~6-10 lines that capture:
- Top-level layout (what containers, headers, sections it renders in order)
- The styling system in use (Tailwind classes, shadcn primitives, CSS-in-JS, custom utility classes like v2-card)
- Any custom inline styles or color tokens
- Interactive elements (forms, dropdowns, tables, dialogs, accordions)
- Any data/state it manages and how it talks to the backend (REST routes, direct DB client, realtime subscriptions)
- Mobile vs desktop responsive considerations visible in the code

Files to inventory (read with the Read tool, do NOT modify any of them):

ADMIN:
1) [absolute path]
2) [absolute path]
...

CONTENT:
N) [absolute path]
...

PAGES:
M) [absolute path]
...

Also:
- List the shadcn UI primitives present in [src/components/ui/] — just names.
- Note any global feature gating like [v2-* utility classes, theme-switching mechanisms, RealtimeProvider patterns] used.
- Briefly note [one or two screen-specific behaviors I need to know about — e.g., search debounce, vote flow, chat message UX, setup wizard step shape].

This is a READ-ONLY task. Do not modify anything. Return a structured, scannable report — file path then bullets, grouped by area (Admin / Content / Pages / Shared / Auth / shadcn primitives / Misc notes). Keep it concise but complete; aim for ~3500 words total max.
```

---

## What to ask for, by area

**App shell + layout files** — every one. The shell determines header, sidebar, mobile nav, and any role-based gating. You cannot plan a redesign without knowing the shell.

**Auth + setup pages** — every one. These are often off-system (hard-coded hex, raw inline styles) because they were built before the design system existed.

**Each top-level route** — every one. Don't sample. The redesign plan has a §5 entry per route.

**Shared components used by multiple routes** — yes. Especially: notification surfaces, chat clients, anything that hooks into realtime.

**API routes** — no. The plan promises not to touch them; you only need to *know they exist*, which the route listing gives you for free.

**Database migrations** — no. Out of scope.

**Library files (`lib/`, `utils/`)** — only if they appear to drive UI behavior (formatting helpers, period calculation, etc.). Otherwise skip.

---

## What to do with the audit output

The subagent returns a structured report. Treat it as a working document for your context:

1. Scan for **partial-migration smells**: components in a different palette than the rest of the app (light vs dark), raw `bg-[#…]` hex tokens, off-system gradient backgrounds. These become P0 regressions in §6.

2. Scan for **already-good components** that don't need much: tables already styled with the design tokens, forms already using the input utility classes. These become "visual only" / "no change" entries in §8.

3. Scan for **realtime subscriptions**: every channel name + filter shape. These go verbatim into §9's "behavior preserved" notes for the relevant screens. The plan promises not to touch channel names or filters.

4. Scan for **dialogs / sheets / popovers**: these are the high-risk surfaces in a restyle (lots of nested context, focus management, ARIA roles). Default to keeping the shadcn primitive and only restyling its content.

5. Scan for **role-gated branches** in components (`isAdmin && ...`). Note them; the plan must preserve these conditionals.

---

## What not to do

- Don't ask the subagent to *also* draft the plan. The subagent doesn't know the user's Phase C decisions; the plan will be wrong. The subagent's only job is the audit.
- Don't ask for snippets or code blocks. Plain-prose bullets per file. Snippets bloat the response.
- Don't accept a free-form essay. Insist on per-file bullets so you can grep your way through it.
- Don't run multiple audit subagents in series unless the file list is too long for one. (~25–35 files per agent is the sweet spot.)

---

## Sanity-check the audit before writing the plan

Before you start drafting §5 of the plan, check the audit for:

- **Coverage:** every file you sent appears in the report. If something is missing, ask the subagent to extend.
- **Consistency:** the audit's vocabulary matches the reference's vocabulary. If the reference talks about "rewards" and the audit talks about "events", note the mapping somewhere obvious so the plan uses one set of terms consistently.
- **Realtime:** every realtime channel name + filter shape is captured. Missing realtime detail is the most common cause of redesign-induced regressions.
- **Role gating:** every admin-only branch noted.

If any of these are weak, ask the subagent for a follow-up pass with a sharper prompt rather than guessing your way through.
