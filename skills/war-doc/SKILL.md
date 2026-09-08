---
name: war-doc
description: Use when Alex asks to "/war-doc", "wargame this", "spec it wardoc style", or hands over a multi-step build, feature set, or integration with business unknowns before code exists — and whenever an agent is about to start building from a verbal ask with no move-by-move plan, no named unknowns, and no live-surface check defined. Also use to bootstrap `wardoc/` in a project that has none.
---

# /war-doc — wargame before build

## Overview
A plan assumes every move works. A wargame writes down, per move, what you will SEE if it worked,
what you'll see if it didn't, and what to do then; every business unknown becomes a named variable
Alex rules on BEFORE code; the last move is always Alex checking the live surface. Alex's verdict
(2026-08-20, Service Agreement): "worked so much better than any other build."
Extends `~/Tidied/Docs/WarDoc_BluePrint.md` (the template) with the execution ritual that made it
work. Sits beside /spar: /spar
makes a one-page Done list for a single /goal; /war-doc is for missions with several moves and real
unknowns.

## The recipe (the output IS these artifacts, in this order)
1. **Capture the ask verbatim** into the project's `INTENT-EVIDENCE.md` (dated, source-tagged) and add
   an ACTIVE sub-goal line to `INTENT.md` before anything else.
2. **Bootstrap once per project** — if `wardoc/` is missing, copy `templates/` from this skill:
   `_WARGAME_ORDER.md` (rules + project standing orders), `CONFIG.md`, `SUCCESS.md`, `LEDGER.md`,
   `tasks/`, `wargames/`, `proof/`. Fill the `{{PLACEHOLDERS}}`; never leave them.
3. **Recon with numbers.** Measure the real system (row counts, distinct values, existing schema,
   existing UI seams, what already works) and write a "Recon facts" section. Facts carry counts;
   assumptions are not facts.
4. **Write `wargames/<mission>.md`** from `templates/wargame-template.md`. Every Move has ALL of:
   action · expected observation · failure observation · counteraction · fork triggers
   ("if X → route A") · 3rd-order consequence. Then: Design (decided things), **Variables flagged**
   (each `(NAME)` with a recon lean), Abort conditions, Self-verification pass whose final move is
   **Alex's own check at the live surface with real data**. Check it against `SUCCESS.md`.
5. **Open every variable in `LEDGER.md`** with its lean. Ask Alex **one variable per message**: a
   concrete example or table, a recommendation, the consequence in one sentence. Record her answer
   **verbatim** in LEDGER (strike the variable through) and in INTENT-EVIDENCE.md. If she says she
   doesn't understand, re-explain with a named-customer example before re-asking. Build on a lean
   only when the wargame's fork says the move may proceed unruled.
6. **Execute** on a branch: dispatch builders with named-file scope and "≤2 self-QA rounds, then
   report residuals"; each builder returns raw evidence (command output, drive steps PASS/FAIL,
   screenshots into `wardoc/proof/<mission-date>/`). The orchestrator reads the code AND the
   screenshots, re-checks the rulings in the implementation, and fixes or re-dispatches what the
   builder missed. Preview deploy → present the branch to Alex with proof → merge on her approval →
   production → **her live check closes the mission**. Flip the INTENT ledger line to DONE.
7. **Tail asks** (her fix-ups after the live check) get the same loop in miniature: capture verbatim,
   one ruling if needed, verify on preview, ship.

## Quick reference
| Artifact | Lives at | Owner of truth for |
|---|---|---|
| `_WARGAME_ORDER.md` | `wardoc/` | rules of engagement + project standing orders |
| `wargames/<mission>.md` | `wardoc/wargames/` | moves, forks, abort, verification |
| `LEDGER.md` | `wardoc/` | every `(VARIABLE)` and Alex's verbatim ruling |
| `proof/<mission-date>/` | `wardoc/proof/` | screenshots/outputs a later agent can re-check |
| INTENT ledger + EVIDENCE | project root | sub-goal status and her words |

## Common mistakes (seen in baseline testing)
- Reading the ask and starting to build "the obvious part" — no wargame, no observations, no variables.
- Leaving an unknown as a paragraph in a report instead of a `(VARIABLE)` with a lean and one question.
- Bundling three questions into one message; answering them for her with "(Recommended)" options.
- Calling green tests "done"; the mission's last move is a human at the live surface with real data.
- Working outside the named scope (another agent's branch, the live repo during a test) — stay in the
  files the move names; a builder that roams costs more than three that don't.
- Retiring a ruling because it is old or quiet — rulings are struck through only by a newer ruling.
