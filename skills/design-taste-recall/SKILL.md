---
name: design-taste-recall
description: Use when any task has a visual surface Alex will see or judge — building/restyling UI (web, iOS, dashboard, tool panel, artifact page), generating images/concept art/video/3D, designing documents, slides, diagrams, game art, or physical objects — BEFORE writing any design brief, blueprint, mockup, or dispatching builders on visual work. This includes MID-SESSION restyle/refinement asks on one section or component ("make this look like <inspiration>", "match this mockup", "make it more visually appealing", or she is unsatisfied with how a delivered surface looks) — recall fires BEFORE the re-style, not only at project start. Also use the moment Alex reacts strongly to a visual artifact ("wow/amazing/perfect", "so bad/awful/ugly", or gives up with "this is fine"), and when the session designs a surface type she has never designed before (first-of-kind), to capture the evidence.
---

# design-taste-recall — pull Alex's documented taste before visual work; capture reactions after

## Overview

Alex's design taste is documented, evidence-based, and binding. Guessing a look, or building from generic defaults, is the documented failure mode (a first-round design flatly rejected; a dashboard called "so ugly"; her own words: "Why are my agents not using the ui-ux-pro-max skills... I got these skills so that the agents outputs would be better"). This skill is the equivalent of a design-taste skill used from a different tool, whose every use correlated with her strongest "This is amazing / This is perfect" reactions.

Extends: `design-dna` (extraction) and the CLAUDE.md Web/UI design default — this skill is the recall + capture layer for ALL visual media, not just web.

## Recall (before any visual decision)

The design knowledge lives in the `.claude/design-taste/` folder in your home directory (Windows: `%USERPROFILE%\.claude\design-taste\`). Files inside: `DESIGN_TASTE.md` (curated corpus distillation, per-surface modes, pass/fail bars), `DESIGN_TASTE_EVIDENCE.md` (dated verbatim reactions — the tiebreaker), `DESIGN_BRIEF_RULES.md` (how to write visual briefs for builders), `examples/` (reference images named by mode), `blueprints/` (extracted DESIGN_BLUEPRINT.md files).

Read, in order — then pick ONE matching mode, never blend:

1. `DESIGN_TASTE_EVIDENCE.md` — her verbatim reactions by use case/media type, dislikes table, skill track record. **Dated evidence here is the tiebreaker.**
2. `DESIGN_TASTE.md` — curated corpus distillation; pick the per-instance mode matching your surface.
3. Check the project for ANY art-direction doc — `DESIGN_BLUEPRINT.md`, `design-system.json`, `ART_DIRECTION*.md`, or equivalent — not just that literal filename. It binds **for the surface it covers**. If your surface is different (e.g. the doc specs the app UI and you're building its marketing page): inherit the product's identity tokens (base colors, accent, type, signature components) and pick the corpus mode for YOUR surface — identity travels across surfaces, layout/mode does not. If CLAUDE.md names a personal-brand design system, it supersedes both files where they conflict.

If the design-taste folder is not found: say "design-taste folder not found" once and proceed using the inline non-negotiables + pass/fail bars below.

"Pick ONE mode" means one corpus mode per surface — the project's own identity tokens always compose with it; what's forbidden is blending two *corpus modes* on one surface.

Then route the work through the specialist packs instead of hand-rolling: `ui-ux-pro-max:*` for style/palette/typography/design-system decisions, `gsap-skills:*` for any animation/scroll/timeline work, `dataviz` for charts, `higgsfield-generate`/`meshy` for image/3D assets (never CSS-placeholder art).

Non-negotiables regardless of mode: one accent per surface · real/photoreal imagery over fake-looking or decorative stand-ins · calm purposeful motion (no stills where aliveness is the point) · no emoji in UI · glanceable density with key status visible · render-and-show before treating a look as approved — her eye outranks any metric. Fold in these pass/fail bars when the folder is missing:

- **Depth test:** a screen with no layering, blur, or shadow depth fails — aim for at least two depth planes.
- **Accent test:** count saturated hues doing "look at me" work; more than one per surface fails (a full-screen gradient counts as the surface, not the accent).
- **Imagery test:** any hero/feature area filled by a CSS gradient blob or generic SVG where a photo/3D render/product shot could exist fails.
- **Numeral test:** a data tile whose key number isn't readable at a glance (huge, high-contrast) fails.
- **Softness test:** sharp-cornered, tightly-packed, thin-bordered boxes fail — radii are large, padding generous, shadows diffuse.

## Capture (the living record)

When Alex reacts strongly to a visual artifact this session — LOVED ("wow", "amazing", "perfect", "so good"), DISLIKED ("so bad", "awful", "ugly", "embarrassed"), or SETTLED (back-and-forth ending in "this is fine") — append ONE line to the nearest matching section of `DESIGN_TASTE_EVIDENCE.md` **in the same session**: verbatim quote (typos kept) + date + project + what the artifact looked like. Add a new subsection only if the entry fits no existing one — nearest-fit beats taxonomy-perfect. Update the recall patterns above only if the entry genuinely changes one. Append, never rebuild. Re-read the file's tail immediately before appending — the file is not tracked centrally and a sibling session can wrap in the same minute; append against the fresh tail, never against the copy read at Recall time.

**Novel surface = capture regardless of reaction strength.** When this session designed a specific artifact-type with no existing ENTRY in `DESIGN_TASTE_EVIDENCE.md` (her first badge system, first 3D-printed object, first slide deck…), record it anyway. Check entries, not section headers — the sections are broad buckets, and a novel artifact-type inside an already-populated bucket still counts: what it was, what she chose/rejected/settled on, and her verdict. First-of-kind evidence is the record's only coverage of that medium until she reacts again — remembering to capture it manually is exactly what fails without a standing rule.

**Rides /wrap, like /retro.** At wrap of any session where a visual surface was built or judged, run this Capture step before closing (or state its explicit skip: "no uncaptured reactions"). Never rely on Alex remembering to invoke capture herself.

## Red flags — stop and run recall

- "I'll just make it clean and modern" / styling from memory
- Alex asked to refine/restyle an existing section mid-session and you're editing styles without having run recall this session — "it's a small tweak" is a documented failure mode (a badge and a window mismatch both missed this way in one session)
- Writing a design brief, BUILD_BRIEF, or builder dispatch with no taste-source citations
- Applying a named aesthetic (macOS, a reference site) to every layer instead of the layer she bound it to
- A strong reaction just happened and you're moving on without capturing it

| Excuse | Reality |
|---|---|
| "This is a small/internal UI" | An internal-only tool panel drew the same complaint anyway: "ugly and difficult to decipher" |
| "The reference speaks for itself" | References filter through her taste (design-dna step 1); one reference was applied raw and had to be reversed |
| "I'll capture reactions at wrap" | Wrap loses the artifact description; capture takes 60 seconds now |
