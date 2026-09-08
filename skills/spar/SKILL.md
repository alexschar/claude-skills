---
name: spar
description: Interview Alex into a one-page, verifiable PRD using the SPAR framework (Set context → PRD → Aim → Restrict), so a later /goal run terminates cleanly against a checkable Done list instead of wandering and burning tokens. ALWAYS use when Alex says "/spar", "spar prd", "write a PRD", "make a PRD", "let's spec this out", "interview me about this project", "help me scope this before I build", "set up a /goal for this", or is about to point /goal at anything non-trivial without a written spec. Also use when a build keeps drifting because the goal was never made verifiable.
---

# /spar — interview Alex into a verifiable PRD (SPAR framework)

**Purpose:** `/goal` only terminates cleanly when its success condition is *checkable*. "Work on the dashboard" never finishes; "every item in the Done list passes its verification command" does. This skill runs Alex through the SPAR framework — **S**et context, **P**RD, **A**im, **R**estrict — one question at a time, and lands a one-page `PRD.md` whose Done section becomes the `/goal` condition. The whole point is to spend a little structured thinking now so a `/goal` run later doesn't wander, re-explore, or drift.

Works for any project — new build, refactor, bug fix, script, data work, existing code or greenfield.

This skill is the operating procedure; the SPAR framework long-form write-up is not required to run it.

Run the steps in order. Do not skip the one-question-at-a-time rule — it is what makes the answers sharp.

---

## S — Set Context (before any questions)

Prime yourself on the material so your interview questions are specific, not generic, and so you can check the repo yourself instead of making Alex guess.

- **Existing project:** read the codebase — tech stack, folder layout, data flow, conventions. Skim the actual files; don't infer from names. If a `CLAUDE.md`, `STATUS_AND_NEXT_STEPS.md`, or `INTENT.md` exists, read it first.
- **New project:** read whatever Alex gathered (mockups, notes, API docs, screenshots). Name what's still ambiguous before writing anything.
- **Persist it** so priming is never paid for twice: offer to save a ≤40-line brief (stack, layout, data flow, conventions) to `CLAUDE.md`.

You've done Set Context well when you can ask project-specific questions and resolve factual answers yourself by looking.

## P — PRD (the interview)

Interview Alex, then write the PRD. **Rules that make this work:**

- **One question at a time.** Ask, wait for the answer, then move on. Never batch.
- **Push back on vague answers.** If an answer is fuzzy or skips the topic, ask the sharper version before advancing.
- **Check the repo yourself.** When an answer is a fact you can verify (does a file exist? what's the stack? is there already a system doing this?), go look and confirm in one line rather than asking Alex to recall.
- **Draft answers for her.** Alex moves fast. For each question, offer a concrete draft or lettered options (a/b/c) built from what you already know, and let her confirm, pick, or correct. "Take your defaults" is always a valid answer — honor it.
- **Watch for scope conflicts and surface them the moment they appear.** If a later answer contradicts an earlier one (e.g. "upgrade the existing system" then "build it as a native app"), stop and re-square both together — don't quietly let the project change shape. This is the highest-value thing the interview does.
- **Don't write the PRD until all seven topics are answered.** When you have what you need, say "I have what I need" and produce it.

Ask these seven, one at a time. Adapt wording to the project type; each must cover its topic:

1. **SCOPE** — "In one sentence, what are you trying to accomplish? Is this a new build, a change to existing code, or something else?" *(Resolve new-vs-existing explicitly — check the repo for prior art before accepting "new".)*
2. **WHY & BAR** — "Who or what is this for, and what does success look like *in use* — not in checks? Then name the 2–3 qualities that would make it done *well*, not just done, and for each, how you'd recognize it by looking." *(This is the counterweight to minimal-viable-output: a model given only a checkable Done list builds exactly to that floor. Push each quality toward an observable proxy — "feels fast" → "the list filters as I type", "looks finished" → "a screenshot next to [reference] doesn't embarrass it". Bars, not ingredients.)*
3. **STACK** — "What language, framework, or tools are involved? If it's existing code and you're not sure, say so and I'll check the repo." *(Flag any decision the scope forces — e.g. "you want to interact with it, but a static page can't take input.")*
4. **SURFACES** — "What concrete things will exist or change when this is done? Files, functions, endpoints, CLI commands, pages, tables, hotkeys, notifications — anything a person could point at." *(Draft the list from context; make her correct it.)*
5. **DATA** — "What inputs does this take and what outputs does it produce? Include what's stored, what's read from elsewhere, and any data shapes that matter." *(Nail the write-back / source-of-truth rule if the thing is interactive.)*
6. **CONSTRAINTS** — "What must NOT change or break? For new builds, what are you explicitly cutting from v1? For existing code, what behavior must be preserved exactly?"
7. **DONE** — "How will we know this is finished? List every distinct thing that must be true, and for each, how I'd verify it — a command to run, a file to check, a behavior to test. What seed data should exist so the check is meaningful?" *(This is the most important answer — see the Done bar below.)*

### The Done bar (non-negotiable)

Every Done item must be checkable by someone who didn't build the thing. Each item states **one true condition + exactly how to verify it** (a command, a file, or an observable behavior) + any **seed data** the check depends on. "Works well" is not a condition. If an item can't be verified, rewrite it until it can — an uncheckable Done item is why `/goal` runs forever.

**The defining-loop test (added 2026-07-11, from the Bridge v2 rebuild):** before finalizing Done, ask: *"can every item pass without the product's defining loop being performed end-to-end from the thing we're building?"* If yes, the list is written wrong — add the loop as its own Done item. Two corollaries: (a) if the product **emulates an existing product** (a GC2-style panel, a Notion-like editor…), deep-research the real product's docs BEFORE writing the PRD and map Done items to its primary workflow — Bridge v2 passed 14/14 items (including a signed feel gate) while the panel never did GC2's select-actor→attach→play loop, because seed data let every item pass without it; (b) if an `IMPROVEMENTS.md` or audit findings are in hand, each ranked finding is explicitly adopted into the PRD or moved to Considered-and-rejected with a reason — never silently dropped.

### PRD output structure

**Before writing:** read any user-priors file the project or CLAUDE.md points to, if one exists.

**Grounding gate (2026-08-12):** the PRD is a new deliverable artifact — the PRD-writing reply opens with the `Grounding check — sources: … · research: …` line (sources consulted and research done, one line).

After all seven answers, write the PRD to `PRD.md` in the project root, in exactly this shape:

```markdown
# [Project Name] PRD

> One page. The Done section is the /goal condition — every item verifiable by someone who didn't build it. The Why & Bar section is what the final self-review passes against.

## One-Liner
One sentence: what this is, new build or change.

## Why & Bar
Who it's for and what success looks like in use. Then 2–3 named qualities that make it done *well*, each with an observable proxy.

## Stack
Languages, frameworks, tools. Versions only if they matter.

## Surfaces
Every concrete thing that will exist or change.

## Data
Inputs → outputs. Stored where, read from where, shapes that matter. Write-back / source-of-truth rule if interactive.

## Constraints
Must not change or break. Explicitly cut from v1.

## Done
Numbered checklist. Each: one condition — Verify: command/file/behavior — Seed data if needed. No uncheckable items.
```

Keep it to one page — compress her answers, don't transcribe them. End the PRD with the ready-to-fire Aim and Restrict blocks (below), pre-filled for this project.

## A — Aim (the `/goal`)

Aim at an **end state**, not an activity. The PRD's Done section already is the condition — point at the file rather than pasting it (the file survives context compaction; a giant pasted prompt doesn't). Give Alex this, edited to fit:

```text
/goal Ship [One-Liner from PRD.md]. Done means every numbered item in the Done
section of PRD.md is true. Verify each item exactly the way PRD.md specifies —
run the commands, check the files, test the behaviors — and keep going until all
pass. The Done list is the floor, not the target: once it passes, reread the
Why & Bar section of PRD.md, look at what you actually built (run it, screenshot
it), and fix anything that falls short of the bar before declaring done. Respect
everything in the Constraints section.

Execution routing: you (Fable) plan, orchestrate, and audit only — do not write
implementation code yourself. Dispatch each Done item (or coherent group of
items) to the `builder` agent (Sonnet 5 @ xhigh) with the spec chunk plus these
Restrictions. Review each returned diff and its verification output against
PRD.md and the Why & Bar section before dispatching the next chunk. When a
builder returns a QUESTION, answer it via SendMessage to that same agent — don't
respawn. Send review fixes back to the builder rather than editing yourself.
Delegate in Done-item-sized chunks, not single edits. Independent chunks go
out in parallel — don't throttle builder count for token cost (count is
second-order to tier + scoping) — but every parallel builder carries a
named-file scope and an explicit QA budget; dependent chunks stay sequential.
The final Why & Bar self-review is yours, not the builder's.
```

Aiming rules: **one goal per run** (if Done splits into unrelated halves, that's two runs); **every Done item checkable**; **reference the file, don't paste it**; **Done is the floor, Why & Bar is the target** — the final self-review passes against the bar, not just the checklist; **Fable directs, Sonnet builds** — the routing paragraph ships with every Aim block, and in Workflow fan-outs build-stage `agent()` calls get `{model: 'sonnet', effort: 'xhigh'}`.

## R — Restrict (fence the work)

Wandering is what burns the weekly token budget. Give Alex a Restrict block to paste with the `/goal`, pre-filled from the Constraints and Surfaces sections:

```text
Restrictions:
- The deliverable is [state the shape the output must take — e.g. "one firm
  recommendation, not a survey of options" / "a working page, not a plan" /
  "a finished draft, not an outline"].
- Only touch: [folders/files in bounds]. Everything else is read-only.
- No new dependencies without stopping to ask first.
- Do not refactor, rename, or "clean up" anything unrelated to the goal — list
  problems at the end instead.
- Do not change [schema / API contracts / auth / CI] unless the PRD says so.
- Preserve exactly: [behavior from Constraints].
- Prefer the cheapest verification first (one command / one file) before full builds.
- If completing the goal seems to require breaking any of these, stop and explain
  instead of working around it.
```

The pattern: name what's **in bounds**, name what's **frozen**, leave an **escape hatch** so Claude stops and asks instead of tunneling through a wall.

---

## When you're done

Confirm the three deliverables and hand off:
1. `PRD.md` written to the project root, one page, Done list fully checkable.
2. The Aim (`/goal`) block, pre-filled.
3. The Restrict block, pre-filled.

Then flag any calls you made for Alex — a placeholder project name, a shortcut/keybinding you changed to avoid a collision, a scope conflict you resolved — so she can override. `/goal` runs from a SPAR PRD run on Fable, which orchestrates and audits while the `builder` agent (Sonnet 5 @ xhigh, if a builder agent is configured) does all implementation (Alex's calls, 2026-07-08/09: Fable-quality direction without Fable-priced typing). Ask whether to start the build now or leave it for a fresh session. Don't start building unless she says to.

## Failure modes this skill prevents

| Symptom | Missing step |
|---------|--------------|
| Claude re-explores the repo every session | S — persist the brief to CLAUDE.md |
| `/goal` runs long, never quite finishes | A — Done items weren't checkable |
| Drive-by refactors of files never mentioned | R — no fence |
| "Done," but it doesn't do what she meant | P — scope/Done never sharpened |
| Passes every Done item, still "not quite what I wanted" | P — Why & Bar never captured (built to the floor); A — no self-review against the bar |
| Passes every item + a signed feel gate, then an audit forces a rebuild | P — Done never required the product's defining loop (seed data let items pass without it); S — the emulated product was never deep-researched before the PRD |
