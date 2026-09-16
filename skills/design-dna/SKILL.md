---
name: design-dna
description: Use when Alex points at a website, screenshot, or app she loves and wants its look distilled for her own build — "extract the design", "I love this site's vibe", "make ours feel like this", "get the design DNA" — or when a web build (especially a fan-out /goal) is about to start with no written aesthetic bar. Produces DESIGN_BLUEPRINT.md; it does not build anything.
---

# design-dna — extract a reference site's design into a reusable blueprint

## Overview

Turn a website Alex admires into a `DESIGN_BLUEPRINT.md` that states quality **bars** (pass/fail sentences) and **invariants**, not ingredient lists. The blueprint is the durable artifact; it feeds `/spar` briefs and fan-out `BUILD_BRIEF.md` files. For applying a reference to an *existing* app, use `ui-redesign-from-reference` instead — this skill is the extraction front-half.

## Procedure

1. **Taste first.** Read `DESIGN_TASTE_EVIDENCE.md` and `DESIGN_TASTE.md` from the `.claude/design-taste/` folder in your home directory (Windows: `%USERPROFILE%\.claude\design-taste\`) — dated evidence in `DESIGN_TASTE_EVIDENCE.md` is the tiebreaker; `DESIGN_TASTE.md` holds the curated corpus, pick the matching per-instance mode — plus any saved user taste-profile notes and the current project's INTENT.md/brief before looking at the reference. If no INTENT.md/brief exists, record that as a `TBD:` line in Source & stance and proceed. The blueprint is the reference *filtered through Alex's taste*, never a raw clone. Never import another project's theme tag wholesale — a mode built for one project's brand is not a universal skin. If Alex gave only a URL, ask her one question — "what specifically do you love about it?" — if she's available this session; otherwise write the stance as `assumed, pending Alex's confirmation`.
2. **Capture the reference.** WebFetch for structure and copy hierarchy; claude-in-chrome for a desktop screenshot AND computed styles (via javascript_tool: `getComputedStyle` on body/h1/buttons for real font families, sizes, colors). For mobile truth, headless Chrome `--window-size=390,2400 --virtual-time-budget=15000` (claude-in-chrome cannot shrink the CSS viewport). If given screenshots instead of a URL, map them tile-by-tile — every tile is kept / replaced / dropped-because; silently dropped tiles are a known failure.
3. **Write `DESIGN_BLUEPRINT.md`** in the project root. ALL sections below are REQUIRED slots — an empty slot (or missing input) is written as `TBD: <what's needed>`, never omitted. Values not observed this session (fonts, hexes, timings recalled from training rather than captured) are labeled `unverified direction` — never presented as captured:
   - **Source & stance** — reference URL, what Alex specifically likes about it, what we are explicitly NOT taking.
   - **Typography** — families with fallbacks, scale (px/rem), weights, where each is used.
   - **Color** — hex values with roles and rough proportions (dominant / secondary / accent).
   - **Layout grammar** — container width, spacing scale, section rhythm, grid logic, density.
   - **Motion grammar** — what animates, easing, duration, and the end-state rule: every animation settles to a state where 100% of copy is legible; test the end-state, not the motion.
   - **Imagery rules** — photo vs illustration vs generated; for ANY hero/background image: crop, background-position, and focal band specified in writing. Prefer a clear photo/generated asset over a muddy bespoke SVG.
   - **Quality bars** — one-sentence pass/fail per visual element type (e.g. "hero readable in 3 seconds with motion off; if it needs a caption to parse, it fails").
   - **Global invariants** — a verbatim-pastable block for fan-out briefs: text never occluded or clipped; framing specified on every handed-off image; no templated tells (default Inter, purple-gradient-on-dark, cards-in-cards).
4. **Extract principles, not property.** Never copy copy, logos, imagery, or distinctive trade dress — typography systems, palettes-as-direction, spacing, and motion grammar only.
5. **Hand off.** End by naming where the blueprint plugs in (the /spar brief or BUILD_BRIEF) and remind that the post-build screenshot sign-off gate (desktop AND mobile) still applies — a blueprint does not replace Alex's eyes.

## Common mistakes

| Mistake | Fix |
|---|---|
| Listing ingredients ("must have animation + SVG") | Every element gets a pass/fail bar |
| Skipping the taste profile because the reference "speaks for itself" | Taste profile is step 1, unconditionally |
| Hex-dumping 30 colors | Roles + proportions, max about 6 values |
| "Use the image for the hero" with no framing | Crop/position/focal band or it doesn't ship |
