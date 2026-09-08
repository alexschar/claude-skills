---
name: gap-check
description: Hunts for what's ABSENT from a plan, decision, or wrap-up under discussion in the current conversation — not what's wrong with what's present. ALWAYS use when Alex says "/gap-check", "what are we missing", "look further", or wants missing spots found in a plan that's still being decided, still forming, or being wrapped up. NOT adversarial review of an existing artifact's correctness (that's /fact-check), NOT a project-wide improvement audit (that's /improve), NOT the full pre-build plan interrogation before implementation starts (that's plan-interrogator).
---

# /gap-check — omission hunt over the plan on the table

**Purpose:** Find the spot nobody named. /fact-check asks "is what's here correct?" /gap-check asks "what's missing from the frame entirely?" — the mechanism nobody wired a trigger to, the population the plan forgot exists, the step everyone assumed someone else owns.

**Origin case (the canonical illustration — use it when explaining what this skill catches):** a session shipped a git-commit protocol for the harness. Every piece built tested green. Nobody asked "what GUARANTEES a commit ever happens?" — the wrap ritual was never amended to call it, so the protocol relied on agents volunteering to run it. The gap surfaced only because Alex asked a boundary question the plan itself never posed. That is the shape of every finding this skill should produce: not "step 4 is wrong" but "there is no step 4."

Run in order.

## 1. Restate the plan's declared scope (one line)

Before hunting, pin down in one sentence what the plan under discussion claims to cover. This is the frame; every gap-class walk below asks "does the scope actually reach this, or does it just sound like it does."

## 2. Walk the eight gap classes against the scope

For each class, ask the question below against the actual plan text / decisions on the table. Don't skip a class because it "doesn't seem to apply" — a class with nothing to report is itself a data point (say so explicitly), but the walk has to happen for the run to count. Write each class concretely enough that a mid-tier model can hunt it cold:

1. **Invocation guarantees** — For every new mechanism, rule, or protocol the plan introduces: what forces it to run? Name its actual trigger points (a hook, a cron, a ritual step inside an existing skill/process, a hard gate in code, a CI check). If the only thing that makes it fire is "an agent is supposed to remember" or "someone will run it," that's a gap — write down exactly what's missing (the hook/ritual/gate that doesn't exist yet) and where it should live.
2. **Legacy population** — Does the plan's coverage reach things that already exist under the OLD rules (open sessions, already-deployed instances, old data, pages/records captured before the change), or does it only cover freshly created things going forward? A plan that says "new X will do Y" without saying what happens to existing X is a legacy-population gap. The test: pick one thing that was already running before this plan started, and ask what happens to it — if the plan text doesn't answer, that's the gap.
3. **Lifecycle coverage** — For each component the plan touches, check all four stages: create, modify, crash/abandon, retire. Plans reliably cover create and the happy-path modify (the "happy middle") and skip crash/abandon and retire. Name the missing stage explicitly, not just "lifecycle is incomplete."
4. **Adjacent systems** — What else is affected by this change but never named in the plan — mirrors, adapters, cross-client copies, docs that restate the same fact elsewhere, downstream consumers of the changed data/API/file? If a fact changes in one place, every other place that states the same fact and isn't named in the plan is a gap (same-turn truth propagation is the bar being missed).
5. **Attribution and race windows** — For each action the plan describes, who or what gets recorded as having done it? Are there two actors (two agents, a human and an automation, two concurrent sessions/processes) that could touch the same thing at once? If the plan doesn't say who wins or how the record disambiguates them, that's a gap.
6. **Instruments and markers** — Does the plan touch any file, string, log format, or config key that a live experiment, measurement, or automated check depends on matching exactly? If a rename, reformat, or restructure isn't cross-checked against what depends on the old shape, that's a gap.
7. **Shipped-but-untested paths** — List every path/branch the plan creates or changes. Which of them has an actual test or a cold run behind it, and which only has "the happy path passed"? A green suite proves nothing about a branch nothing has ever exercised — name the specific untested path, don't just say "coverage is thin."
8. **Implied-but-unassigned steps** — Read the plan for actions it assumes will happen — "someone should," "Ops will," "eventually," "once we get to it" — and check each for three things: an owner, a trigger, and a date/condition. Any one of the three missing is a gap; say which of the three is absent.

## 3. Output — glanceable, ranked

For every finding, one line each in this shape:

`[class] what's missing → concrete consequence if left unclosed → cheapest close`

Rank by consequence severity, worst first. State what's absent as a fact, not a question — then the cost of leaving it absent. Note separately any class that was walked and came back clean (say so — don't just omit it).

Findings only. **Implement nothing** without Alex's explicit go — this skill hunts, it does not fix.

## 4. Offer the deeper mode (never auto-run)

If the plan is high-stakes (production data, money, client-facing, or the origin-case pattern — a protocol nothing enforces), offer in one line: dispatched refute-first subagents, pinned Sonnet, one per suspicious class, to go deeper than a single-pass read can. Wait for her word before spawning anything.

## 5. Close

End with a short block that opens with the literal line **"In plain terms:"** — plain-English adult register, no jargon, what's actually missing and why it matters, per her standing communication rule.

## Notes

- Extends /fact-check's adversarial ethos to ABSENCES rather than errors — /fact-check asks whether what's on the page is correct; /gap-check asks whether the page is complete. Skill-ifies the "completeness critic" pattern from her workflow library. Replaces nothing. For a neutral answer to an open question (not absences, not errors) from an agent that did not do the work, route to `/3rd-party`.
- Not for reviewing an existing artifact's correctness (/fact-check), not a project-wide "what would make this better" pass (/improve), not the full pre-build decision-tree interrogation before implementation starts (plan-interrogator) — those ask different questions of different targets.
- A gap class returning nothing is a valid, reportable result; skipping the walk on a class is not — state which classes were actually walked.
