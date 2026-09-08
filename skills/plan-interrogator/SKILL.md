---
name: plan-interrogator
description: Rigorously interrogates a plan, design doc, architecture proposal, PRD, or any decision-heavy plan before implementation begins — walks every branch of the decision tree, surfaces dependencies between decisions, and drives toward genuine shared understanding instead of surface-level agreement. Checks the codebase (patterns, config, dependencies) before asking the user, and only asks what truly requires the user's judgment. Use whenever someone hands over a plan and says "review this plan," "let's break this down," "let's get on the same page," "explain to me what this plan is for / will do / will achieve," "how do you think we should do it," "before I start building X," or shares a design doc/PRD to stress-test before work starts. Trigger even without the words "interview" or "plan" — any handoff of a not-yet-fully-specified proposal should trigger this instead of a quick skim and a nod.
---

# Plan Interrogator

## Purpose

Most plan reviews fail the same way: the reviewer reads the doc, asks one or two questions about the parts that seem underspecified, gets an answer, and says "sounds good." That produces agreement on the parts that were already clear and silence on the parts that weren't — which are usually the parts that break the plan later.

This skill runs a different process. It treats the plan as a tree of decisions, not a single unit to approve. It walks every branch, forces each decision to be explicit, tracks how decisions constrain each other, and only stops when every branch is actually resolved — not when the user seems tired of answering questions. It also refuses to burn the user's attention on anything the codebase can already answer.

## When this activates

Trigger on any handoff of a plan, design doc, PRD, architecture proposal, or "here's what I'm thinking" that precedes real implementation work — regardless of exact phrasing. Do not wait for the word "interview." Do not trigger on requests to execute an already-agreed plan, or on trivial one-off scripts/tasks with no real decision surface.

## Step 1 — Ingest and build the initial decision tree

Read the plan fully before asking anything. Then decompose it into a tree:

- **Root**: the stated goal / problem the plan solves.
- **Branches**: the independent axes of decision the plan touches. Look for architecture/component boundaries, data model and storage choices, API/interface contracts, state management, error handling and failure modes, concurrency/consistency, security and privacy-sensitive choices, third-party dependencies, migration/rollout strategy, and anything the plan states as a conclusion without showing its reasoning.
- **Leaves**: the specific open question or claim under each branch.

A branch belongs in the tree if resolving it wrong would force a rewrite elsewhere. Skip branches that are genuinely cosmetic — this isn't about maximizing question count, it's about covering every point where a wrong guess is expensive.

Write this tree to a live markdown file (see Step 4) before asking the user anything. This first pass is your working hypothesis of the tree, not the final shape — expect to add, merge, or drop branches as answers come in and reveal things you didn't anticipate.

## Step 2 — Resolve what the codebase already answers

For every leaf, ask: *is this discoverable, or does it require someone's judgment?*

Discoverable means you can find the answer (or strong evidence for it) by reading the repo: existing architectural patterns, naming conventions, the frameworks and libraries already in `package.json`/`requirements.txt`/etc., existing data models, config files, test structure, CI setup, prior art for similar features elsewhere in the codebase. If it's discoverable, go explore it — Grep, Read, Glob, whatever it takes — and resolve the leaf yourself. Mark it resolved-by-exploration in the tree with a pointer to what you found (file/line), so the user can spot-check instead of re-deriving it.

Judgment-required means the answer depends on priorities, constraints, or context that live only in the user's head: trade-off preferences (speed vs. cost, simplicity vs. flexibility), business/product constraints, risk tolerance, team conventions not yet reflected in code, or genuine ambiguity the plan itself doesn't resolve. Only these go to the user.

If you're not sure which bucket a leaf falls in, spend the two minutes checking the code first. Guessing the user's intent is worse than a quick grep.

Two nuances that come up constantly:

- **A third outcome: the repo proves something is missing, not answerable.** Exploration sometimes shows the plan depends on something that doesn't exist yet (no migration for a table the plan assumes, no queue infra for a step the plan assumes is async). That's not resolved and it's not a question for the user's opinion either — it's a gap in the plan itself. Mark these `resolved: exploration — gap found`, state what's missing, and treat closing the gap (or explicitly deferring it) as part of the plan, not a footnote.
- **Don't let "resolved by exploration" overclaim confidence.** Sometimes the repo only supports a lean, not a fact — e.g. there's no existing queue infrastructure, which is real evidence for "do this synchronously" but not proof it's the right call for this feature. When exploration only narrows the field rather than settling it, resolve with the lean but say so explicitly (`resolved: exploration — leaning, not certain`) so the user can veto it in one glance instead of having to re-derive your reasoning.

## Step 3 — Interview, following dependencies

Ask the user only the judgment-required leaves, and do it as a real interview, not a survey:

- **Batch related questions.** Don't ask one leaf at a time if three leaves are all part of the same branch — a scattershot one-by-one interrogation is more tiring than a focused one, and it's how you lose the thread of dependencies.
- **Order by dependency, not by document order.** Before asking a leaf, check whether its answer is constrained by an already-resolved leaf, or whether it will constrain leaves not yet asked. If branch A ("do we do this synchronously or async?") determines the answer space for branch B ("what's the retry/idempotency story?"), resolve A first and carry its implication into how you frame B — don't ask them as if independent.
- **Surface the dependency explicitly when it matters.** If the user's answer to one question invalidates or reframes an earlier answer (theirs or one you resolved by exploration), say so and re-open that node rather than quietly letting the contradiction sit in the tree. Concretely: if leaf B is still `open` and leaf A was resolved on a lean that B could overturn (e.g. A: "sync, since there's no queue infra" resolved while B: "what's the max batch size?" is still open and a large batch size would force a queue), mark A `resolved — provisional, pending B` rather than fully resolved. Once B lands, immediately re-check whether A still holds.
- **Push back, don't just record.** If an answer conflicts with something the codebase actually does, or contradicts an earlier answer, say so before moving on. The goal is shared understanding, not a transcript of whatever was said first.

Update the decision tree file after every exchange — this is a live document, not a summary written at the end.

## Step 4 — The decision tree file

Maintain one markdown file for the duration of the interview (see `assets/decision-tree-template.md`). Create it the moment Step 1 produces a first draft, and rewrite it after every meaningful update — new branch discovered, leaf resolved, dependency surfaced, contradiction found. Each node needs: its question, its status (`open` / `resolved: exploration` / `resolved: user` / `deferred`), its resolution (with file/line citation if from code), and any dependency edges to other nodes.

## Step 5 — Exit condition

Stop interviewing when all of the following are true, not when you run out of obvious questions:

1. Every branch in the tree is `resolved` or explicitly `deferred` (a conscious choice, not an oversight).
2. No two resolved nodes contradict each other.
3. Every dependency edge has been checked — resolving the upstream node didn't silently invalidate a downstream one, and no node is still `provisional` with its blocking leaf already answered.

When you believe you've hit this point, say so plainly, name what's left deferred, and confirm before moving to output. Don't ask a marginal 15th question just because you technically could.

## Step 6 — Output

**Before writing:** read any user-priors file the project or CLAUDE.md points to, if one exists.

Ask the user which they want (or infer it):

- **Write a PRD/spec.** If the plan needed real clarification, synthesize the resolved decision tree into a structured document using `assets/prd-template.md`, adapted to what the plan actually needed.
- **Start implementation.** If the interview mostly confirmed an already-solid plan, treat the resolved decision tree as the spec and begin work directly, citing resolved nodes as you go.

Either way, leave the decision tree file in place as the record.

## Step 7 — Write the intent record back (required on both output paths)

The interview you just ran is the densest intent signal the user produces: their typed answers ARE the project's intent, in their own words. The decision tree is a per-plan artifact; `INTENT.md` at the project root is the cross-session record future agents are guaranteed to read — so the typed signal must land there before this skill ends. Do this after Step 6's choice, before implementation begins or immediately after the PRD is written:

1. Collect every node resolved by a **typed** user answer, plus any typed statement from the interview about purpose, success criteria, or constraints even if it resolved no node. (A clicked option from an offered list is weak evidence — include it only if load-bearing, suffixed `(clicked)`.)
2. Append one dated line per item to the project's `INTENT-EVIDENCE.md` (contract v2 — never into INTENT.md itself), **verbatim — never paraphrased**:
   `- [YYYY-MM-DD · interrogation] "exact typed words" (node: <branch>)`
3. If an answer states a new non-negotiable or contradicts the current synthesis ("The intent" / "Non-negotiables"), also append a dated DELIBERATE line to INTENT.md's `## Intent history & pivots` and say so in your closing message — but do **not** rewrite the synthesis sections themselves; that is `/intent`'s job.
4. No `INTENT.md`? Create the vaccinated pair: (1) `INTENT-EVIDENCE.md` with these entries; (2) an `INTENT.md` stub — title, an `## Evidence` section containing ONLY the pointer "APPEND-ONLY archive — lives in INTENT-EVIDENCE.md. Append new entries THERE.", and the line `Synthesis pending — run /intent to write "The intent" from this evidence.` Evidence entries never go inside INTENT.md.

The step is complete only when `INTENT-EVIDENCE.md` contains today's dated entries. "The tree file already records the answers" does not satisfy it — the tree does not follow the project across sessions.

A project `CLAUDE.md` that says "do not edit INTENT.md except via /intent" refers to the **synthesis** sections — dated Evidence/pivot appends from this step are the sanctioned capture path and never violate it. Do not skip Step 7 on that basis.

## What to avoid

- Don't ask the user something you could have found in the repo in under a minute.
- Don't treat every leaf as independent — dependencies are the whole point.
- Don't keep interviewing past genuine shared understanding just to be thorough.
- Don't let the decision tree file go stale.
