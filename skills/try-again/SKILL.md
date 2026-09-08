---
name: try-again
description: Use when Alex says "/try-again", "that is not it", "you did this completely wrong", "that does not match what I gave you / the photo / the example", or otherwise rejects a just-delivered result as not matching her ask. Also fires in loop mode when she asks to refine until it's actually good ("keep refining until it matches", "refine this until it's right", "run the gauntlet on this") or rejects the same artifact a second time. A FRESH unbiased agent compares what she asked for (her reference material is binding spec) against what was delivered, states what's wrong, and the fix is retried against that verdict. NOT for capturing a misreading without fixing (that's /distill), reviewing a claim or document (/fact-check), or process self-audit (/retro).
---

# /try-again — fresh eyes compare ask vs. delivered, then fix it

Purpose: when the delivering agent checks its own work it stays biased — it re-reads its own intent and says "matches." /try-again removes that agent from the judgment. A new agent sees only Alex's ask and the artifact, says plainly what's right and what's wrong, and the fix is driven by that verdict.

## 1. Freeze the evidence (no self-defense)

Collect, without adding your own interpretation or justification:
- **The ask, verbatim** — Alex's original words from this session, plus any reference material she supplied (photo, example site, mockup, spec). Her references are binding spec, not inspiration.
- **The delivered artifact** — file paths, and for anything visual, capture the real surface: capture desktop (1440) and true-mobile (390) screenshots with headless Chrome. Verdicts come from what the surface actually looks like, never from the code that produced it.

## 2. Dispatch the fresh evaluator

One Agent-tool subagent, **Opus-tier minimum (`model: "opus"`), never Haiku** (E25 verifier calibration — Haiku fails judgment comparisons). It receives ONLY the frozen evidence — never the delivering agent's reasoning, plan, or excuses. Its prompt must instruct:

> Compare the delivered artifact against the ask and the reference material. Break the ask into its individual requirements (for a reference image: layout, crop/framing, subject, color/tone, typography, mood — what the reference actually shows). For each requirement report MATCHES or DOES NOT MATCH with the observable evidence (cite the file/screenshot region/passage — no assumptions), and for every miss write one concrete fix directive: what to change, in which file, to what target state. Look at the actual images/surfaces yourself. Do not grade effort or intent — only the artifact against the ask.

## 3. Show Alex the verdict, glanceable

Table: requirement → MATCHES / DOES NOT MATCH → one-line evidence. Fix directives as compact bullets below. No prose defense of the original attempt.

## 4. Try again — fix from the verdict, not the old context

Dispatch a **fresh builder** whose spec is the verdict's fix directives (named-file scope + "max 2 self-QA rounds, then report residuals"). The original attempt's reasoning stays out of the builder's prompt — the verdict is the spec now.

## 5. Re-verify before any done-claim

Re-run the same comparison (step 2 evaluator, same prompt, new artifact). Done-claim only when every item reads MATCHES, confirmed at the human-visible surface (fresh screenshots for visual work). Still missing after 2 fix rounds → stop and tell Alex plainly which items remain off and why, no success framing.

## Loop mode — refine until it wins

Fires when Alex's ask is iterate-to-bar ("until it's actually good", "keep refining until it matches", "gauntlet") or she rejects the same artifact a second time. Default stays one round; loop mode repeats steps 2–5 under these rules, and step 5's two-round stop is replaced by the terminal states below:

- **Fresh evaluator every round.** An evaluator that judged a previous round never judges again — it anchors on its own critique. Same prompt, new agent, Opus-tier minimum. The builder each round receives only the current verdict's fix directives.
- **The reference stays binding every round.** The bar never quietly relaxes to "closer than last time"; requirements are re-judged from the reference, not from the previous verdict.
- **One round, one glanceable verdict table** shown to Alex: requirement → MATCHES / DOES NOT MATCH → what changed vs. last round. No prose between rounds.
- **The loop survives a crash.** At loop entry write `<project>/.tryagain-evidence.md` — the frozen ask and reference paths — and after every round append the round number, its verdict table, and the running no-progress tally. Delete the file only when a named terminal state has been reported — in the same reply that reports it, never later: a file left behind after a reported terminal state reads to the next pickup as an interrupted loop. A `.tryagain-evidence.md` found with no live loop is an interrupted loop: resume at the recorded round with a FRESH evaluator, never a restart from round 1.
- **Terminal states — stop at the first reached and name it in-thread:**
  - **WIN** — every requirement MATCHES, confirmed at the human-visible surface (fresh screenshots for visual work).
  - **NO-PROGRESS** — two consecutive rounds where no requirement flips to MATCHES: stop, report the residual gaps plainly, no success framing. Offer /fact-check in one line — except when the stalled artifact is a judgment-call deliverable (layout, plan, design, recommendation) and the dispute is what SHOULD be built rather than fidelity to her reference: then offer /debate instead (grounded adversarial adjudication, if that skill is installed). In an unattended run (no live human turn — /overnight, /handsoff), skip the offer: NO-PROGRESS is a hard stop, logged on the pickup surface.
  - **ALEX-BRAKE** — she stops it. She is the brake; praise mid-loop ("this is better") is not a stop signal — it usually means keep going.
  - **BUDGET** — her stated round/time limit, or a default cap of 5 rounds when she set none. At the cap with Alex present: show the verdict table and ask whether to continue (a fresh 5 only on her word). Unattended, the cap is a hard stop. Exhaustion is reported as NOT DONE with the remaining gaps, never as a pass.

## 6. Capture the divergence

Alex rejecting a delivery as "not what I asked" is /distill's trigger — run it (or offer it in one line) so the misreading is captured durably, not just this fix.

## Notes

- Extends the family, replaces none: /fact-check (formerly /second-opinion) adversarially reviews a claim/doc/analysis; /debate re-decides an under-grounded judgment call with a challenger, defense, and judge panel; /distill captures a misreading without fixing; /retro audits agent process; /3rd-party gives a neutral answer to an open mid-session question from a fresh agent (no fix loop, no panel). /try-again is delivered-artifact vs. ask, with the fix loop.
- Works on any medium — UI, image/3D generation, documents, code behavior. For visual targets both evaluator passes must look at real screenshots next to the reference, side by side.
