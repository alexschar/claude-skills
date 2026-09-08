---
name: bite-size
description: Use when Alex says "/bite-size", asks to keep updates bite-size, says she is fried or fatigued, or wants a build update, plan, recommendation, or status broken into small, easy-to-scan pieces. Not for a one-off plain-language translation unless she also asks for bite-size delivery.
---

# Bite-size

## Core principle

Preserve truth and decisions while reducing reading effort. Reshape the record for
one phone-screen read; never make it vaguer.

## Output contract

Write 2–5 chunks separated by blank lines. Each carries one idea in 1–2 sentences
and begins with **one bold point**. Aim for 150 words; accuracy and blockers outrank
the target. State each fact once.

Choose only the chunks the request needs, in this order:

1. **What matters now** — lead with the outcome, recommendation, or honest status.
   Use a user scenario only when it clarifies the payoff.
2. **Why this direction** — connect the request to the choice and reason. Use past
   tense only for work that happened.
3. **Proof and gap** — separate observed proof from what is unverified, blocked,
   broken, or not started. Tests and service health do not replace a missing real
   workflow check.
4. **Decision** — only when Alex owes one: recommend an option, state its payoff,
   then ask one yes/no question. If the record contains a material risk, state it
   before the question.
5. **Next** — name the next observable action. Use `done → doing → next` when it
   clarifies progress.

## Truth and language rules

- Never invent history, checks, safeguards, counts, completion, or progress.
- Use a percentage or bar only with a real numerator and denominator. Otherwise use
  honest status: done, unverified, blocked, broken, or not started.
- Prefer user-facing names and plain words. Omit paths, internal identifiers,
  agent/model/tool names, and unexplained acronyms unless necessary evidence.
- Keep known numbers. Simplify delivery, never substance.
- Do not narrate routine process. Include reasoning that changes direction, proof,
  risk, or Alex's next decision.

## Quick reference

| Request | Essential chunks |
|---|---|
| Status/update | What matters now → Proof and gap → Next |
| Recommendation | What matters now → Why → Decision |
| Plan | What matters now → Why → Next, with risks in Proof and gap |

## Example

**Not proved on the phone yet.** Service health does not show a reply reaching
Alex's chat.

**The proof run is four steps.** Send a named message, confirm its reply in the same
chat, record the time and provider, then save one screenshot.

**Two outside failures may interfere.** Messaging is degraded and the provider has
rate-limited requests before; record the exact break if no reply arrives.

**Done → doing → next:** service check → phone test → saved evidence.

## Standing mode

"Keep updates bite-size" activates this shape for later updates in the current
session until Alex says to stop.

## Common mistakes

- Forcing a scenario, decision, progress bar, or fixed five-part template.
- Turning a future plan into invented past-tense work or nonexistent safeguards.
- Repeating the same status in both the record and the progress anchor.
- Hiding an unverified workflow behind passing tests or service health.
- Baby-talk, vague summaries, dense paragraphs, or a diary of routine steps.
