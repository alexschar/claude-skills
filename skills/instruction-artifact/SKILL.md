---
name: instruction-artifact
description: Use when Alex asks for a visual instruction doc, walkthrough, tutorial, setup sheet, or step-by-step guide on any topic to hand to someone else — "/instruction-artifact", "make a visual guide for X", "tutorial for [person]", "setup sheet", "show them how to do Y" — especially when the recipient is a non-technical absolute beginner who must get it right on the first try.
---

# Instruction Artifact

## Overview

Turn any procedure into a polished, animated, self-contained HTML instruction doc a beginner can follow unaided — then package it so it actually reaches them. The doc is visually led: a picture of each screen first, one sentence of instruction, one Copy button. Extends `artifact-design` (page craft) with the tutorial recipe + delivery packaging neither it nor `frontend-design` covers.

## When NOT to use

- Internal notes/status docs → plain Markdown.
- UI redesign of an existing app → `ui-redesign-from-reference`.

## Process

1. **Pin three things before building:** the recipient (name them; calibrate every sentence to their floor — for a beginner, explain how to open the app, what a terminal is, what "correct" looks like), the single job of the doc, and the delivery path (see Delivery). Ask only for what's ungatherable.
2. **Invoke `artifact-design`**, sketch a palette/type/layout plan grounded in the subject's world. Both themes, token-based.
3. **Structure — the recipe:**
   - Header: title, one-sentence promise, chips for time/effort ("about 15 min of you, about 90 min of waiting").
   - Group steps into 2–4 named Parts ("Part A · Set up"). Steps are numbered cards: number + verb headline → the one action (bold keys as `<kbd>`) → a **mock-up of the exact screen** → a "What correct looks like" caption naming the observable success signal.
   - Everything the recipient types gets a Copy button block. Pre-fill all content you can; highlight the one blank they must fill and say why only they can.
   - Any "wrong version / right version" risk gets a side-by-side ✓/✗ compare with a one-line check ("see the words X and Y? right one").
   - End with: expected final state (folder tree / screenshot), a "send it back" step, and a troubleshooting section — one fix per plausible failure, each ending in a concrete action ("press Esc, text Alex").
4. **Build — non-negotiable technical rules** (each one is a real defect seen in production use):
   - Scroll-reveal observers use `threshold: 0` — a card taller than about 4 viewports never reaches 0.25 visibility and stays invisible forever.
   - Every animated/JS-gated state needs BOTH `@media print` and `prefers-reduced-motion` overrides forcing the final visible state (revealed cards, typed text at full width, meters at full value), or the PDF/print ships blank.
   - Gate hide-then-animate CSS behind an `html.js` class added by an inline script, so no-JS renders everything.
   - Typewriter spans: `--n` must equal the exact character count or text truncates.
   - Check animated text color against EVERY background it sits on (white terminal-text on a light mock-up card = invisible).
   - Copy in the recipient's vocabulary; pronouns: Alex is she/her — proofread every person reference before delivering.
5. **Verify with eyes, not assumptions:** open the HTML in Chrome; generate the PDF twin and open it with the Read tool's `pages` parameter to look at every page — this catches invisible text and blank sections that source review misses. Cross-check numbers/references against companion docs (email, README) — step renumbering silently breaks "see Step N" elsewhere.
6. **Package & deliver.**

## Delivery

If artifact share links are unavailable on the current plan, ship a **standalone file**: wrap the page in `<!doctype html><html><head><meta charset>…</html>` (the bare-fragment style is only for the Artifact tool), generate a PDF twin via Chrome headless — macOS `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"`, Windows `"C:\Program Files\Google\Chrome\Application\chrome.exe"` — with `--headless --print-to-pdf=<out.pdf> --no-pdf-header-footer <file>`, and bundle HTML + PDF + any companion files into one zip that is the email's single attachment. Name files self-explanatorily (`START-HERE-…`).

## Common mistakes

| Mistake | Fix |
|---|---|
| Sharing the artifact URL when the plan blocks it | Ship the zip instead |
| Verifying only the source/HTML | Read the rendered PDF pages as images |
| Steps assume knowledge ("open a terminal") | Every noun the recipient hasn't met gets one plain sentence |
| Doc updated, companion email not | Grep companions for step numbers, filenames, model names |

Worked examples: `worked-examples/` inside the design-taste folder, if present.
