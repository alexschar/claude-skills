---
name: office-guide
description: Use when Alex asks for a house-style how-to, office guide, setup sheet, or step-by-step page for a coworker or team member — "/office-guide", "make a guide for [person] in the same style as before", "how-to doc for the office", "write this up as a guide with screenshots" — the calm print-first office document with numbered step cards, real screenshots, copy blocks, and a can/can't table.
---

# Office Guide

## Overview

A calm, print-first office document for a real internal process: numbered step cards, real screenshots, who-does-what rows, copy-paste blocks, and an honest can/can't table for the tool in question. Two skills cover adjacent ground and are easy to mix up:

- `instruction-artifact` — an animated, mock-up-led beginner tutorial packaged as a zip, for a recipient who has never touched the tool before.
- `office-guide` (this skill) — a calm, print-first office document built on the shipped house template, with real screenshots, who-does-what rows, copy blocks, and a can/can't table, for a coworker who already works in the tool.

Both share the same delivery rules: verify at the rendered surface (HTML in a browser, PDF page by page), never claim done from source review alone.

## Process

1. **Pin the recipient, the single job, and the delivery path** before writing anything — one guide, one job, one delivery method (HTML + PDF, or HTML + PDF + an email draft).
2. **Write the spec first**, in plain Markdown, before touching HTML:
   - Parts (2–4 named groups of steps).
   - Numbered steps, each with: **Who** (who does it and how long it takes), **Where** (the exact path/tab), **Do this** (the actions, numbered if there's more than one), **Check** (what correct looks like).
   - A `[fig-NN]` marker at every step that needs a screenshot.
   - Copy blocks, lettered (Copy block A, B, C…), verbatim text.
   - A can/can't table for what the tool does and doesn't do here.
   - An "if something is off" table (symptom / why / fix).
   - A "who does what now" table (person / before / after).
3. **Build from `templates/office-guide-skeleton.html`** in this skill's folder (referenced as `templates/office-guide-skeleton.html` relative to this skill). Keep color/type tokens confined to the three `:root` blocks (light, dark-media-query, `[data-theme="dark"]`) — both themes must work. Embed each figure as a base64 PNG with any personal or account-identifying detail cropped or redacted, and mark the one control the step is about with a 3px rounded outline in the accent color. Google Fonts is the only external asset allowed.
4. **The non-negotiable technical rules**, each one a real defect found and fixed in past builds:
   - `overflow-wrap:anywhere` on `code` — a long unbroken token (a URL, a key) otherwise overflows the mobile viewport.
   - `.goal>div` (direct-child selector), never `.goal div` — the loose selector draws nested boxes inside the two intro cells.
   - `break-inside:avoid` on short steps so they never split across a page; headings and `.who` lines keep-with-next; the footer never lands alone on its own page; steps that contain a figure are allowed to split (a screenshot step is often taller than one page).
   - Any table wider than the phone viewport gets a `min-width` on the `table` plus its `.tbl` wrapper's `overflow-x:auto`, so it scrolls inside its own box instead of forcing page-level horizontal scroll.
   - Copy buttons are gated behind `html.js` (an inline script adds the class) so a no-JS render never shows a dead button.
5. **Verify, in order:**
   - Base64 image count equals the number of `[fig-NN]` markers in the spec.
   - Zero hex color literals outside the three `:root` token blocks.
   - Mobile check at 390px width: `document.documentElement.scrollWidth === document.documentElement.clientWidth` (no page-level horizontal overflow).
   - Read every PDF page with the Read tool's `pages` parameter — no split headings, no footer-only page, no invisible text.
   - Proofread pronouns and register; zero emoji anywhere in the document.
6. **Deliver** the HTML and its PDF twin (Chrome headless — macOS `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"`, Windows `"C:\Program Files\Google\Chrome\Application\chrome.exe"` — `--headless --print-to-pdf=<out.pdf> --no-pdf-header-footer <file>`), plus an email draft if one was asked for.

## Common mistakes

| Mistake | Fix |
|---|---|
| `.goal div` instead of `.goal>div` | Nested boxes appear inside the intro cells — use the direct-child selector |
| No `break-inside:avoid` rule | A short step splits mid-card across a page break |
| A wide table forces page-level horizontal scroll on mobile | Give the table a `min-width` and let its `.tbl` wrapper scroll instead |
| Copy button shows with JS disabled | Gate `.copy-btn` behind `html.js` |
| Claiming done from the HTML source alone | Read every rendered PDF page and check the 390px DOM width before delivering |
