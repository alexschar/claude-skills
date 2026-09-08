---
name: fact-check
description: Independent adversarial review without Alex hand-carrying output between agents. ALWAYS use when Alex asks "do you agree with this?", "check their work", "have another agent review this", "review with extreme scrutiny", "critique this", pastes another agent's analysis for a verdict, or wants a diff / document / claim verified before it ships. Spawns refute-first subagents and synthesizes a clear agree/disagree verdict. Formerly named second-opinion (renamed 2026-08-12) — "/second-opinion" or "second opinion" asks route here.
---

# /fact-check — adversarial fact-check review in one place (formerly /second-opinion, renamed 2026-08-12)

Purpose: replace the copy-paste loop where Alex ferries analyses between terminals ("I gave your doc to my other agent, this was their response…"). The independent skeptics run from here; she gets one synthesized verdict.

## 1. Identify the target

One of:
- **A diff / recent change** — `git diff` (or the branch diff) is the target.
- **A document** — customer-facing doc, report, plan, spec (the Lennox pattern).
- **Another agent's analysis** — pasted text or a file; the *claims in it* are the target.
- **A claim** — "X is caused by Y", "this fix works".

If the target references source material (plans, emails, data files, the codebase), locate that material on disk — verifiers must check claims against the actual data, never against the claim's own narrative.

## 2. Spawn independent skeptics (Agent tool)

Calibrate to stakes:
- **Default:** 1 skeptic.
- **"Extreme scrutiny" / production data / customer-facing / money:** 3 skeptics in ONE message (parallel), each with a distinct lens — e.g. factual accuracy vs. source data; internal consistency & completeness; does-it-actually-work (reproduce/trace the logic in code).

Each skeptic's prompt must include, verbatim in spirit:
> Try to REFUTE the following. Your judgements must never be assumptions — only conclusions drawn from the provided data/files. Check every specific claim against the source. If you cannot verify a claim, say "unverified", do not assume it true. Report: each claim → CONFIRMED / REFUTED (with the contradicting evidence) / UNVERIFIED. Cite file:line or the exact source passage for every verdict.

Give each skeptic only the target + pointers to source material — not your own opinion of it (independence is the point).

**Blind visual comparison (adopted 2026-08-07, from the Gauntlet Loop pattern; amended same day after deep gap-check):** when the target includes a visual/feel claim against a reference ("looks like X", "feels like GC2", "doesn't embarrass next to the reference"), hand the skeptic **unlabeled** screenshots/frames of ours and the reference side by side — file names and prompt must not reveal which is which (copy to neutral names like `a.png`/`b.png` in the session scratchpad, deleted after the verdict). The skeptic reports which is better and why BEFORE being told which is ours.

Two honesty limits (both verified the hard way):
- **Blindness only works on same-medium pairs** (two build screenshots, two renders of the same pipeline). A painted concept target next to an in-engine capture self-identifies regardless of filenames — for cross-medium comparisons, drop the blind claim and rely on the two parts that still carry the value: a **non-author judge** and an **explicitly named, Alex-blessed reference**. Never report a cross-medium comparison as "blind."
- Blindness is only as real as the labels: an agent that produced the artifact can't judge it blind, so this leg always goes to a skeptic who didn't build it.

**Evidence durability:** screenshots cited as judged evidence must be copied out of regenerated/gitignored dirs (e.g. Unity's `Library/`) into a durable git-tracked project folder before the verdict cites them — a path that can vanish is not evidence.

**Verifier model routing (adopted 2026-08-05, lab E25 — from the E15 planted-error calibration, measured not assumed):** judgment/pattern-verification skeptics are **Opus-tier minimum (`model: "opus"`), NEVER Haiku** (Haiku 4.5 scored 5/10 on judgment items — it endorsed a confounded finding and passed a double-count it had itself noticed; Opus 5 scored 10/10). Purely mechanical recount/verification legs (counts, greps, checksum-style checks) may run Sonnet or Haiku (all Claude tiers measured 12/12 mechanical). On a contested finding where skeptics disagree, dispatch **Codex as the cross-vendor tiebreak** (measured 10/10 judgment).

## 3. Synthesize the verdict

- Per finding/claim: **agree / disagree / can't verify**, one line of evidence each. If skeptics disagree with each other, say so and which is better supported.
- Lead with the bottom line ("The other agent is right about A and B, wrong about C — here's the proof").
- No hedging, no diplomatic averaging. A wrong claim gets called wrong.

## Notes

- This does NOT replace `/code-review` for working-tree code review — that skill exists and is better for diffs; use /fact-check when the target is a doc, claim, or another agent's output, or when Alex explicitly wants the adversarial "other agent" framing.
- If Alex pastes a rebuttal from her external agent afterwards, treat it as a new target and run the loop again — still no ferrying needed on her side beyond the paste she already made.
- If the review reveals the real dispute is whether a judgment-call deliverable was properly GROUNDED (project sources unread, needed domain research skipped) rather than whether specific claims are true, route to `/debate` — grounded adversarial adjudication extends this skill for that case. If Alex wants a neutral ANSWER to an open question from someone who didn't do the work — not a refutation of a claim — route to `/3rd-party` (if those skills are installed).
