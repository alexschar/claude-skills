# alexschar-skills

A Claude Code plugin packaging 12 self-contained skills for planning, review,
communication, and everyday utility work: `spar`, `war-doc`,
`plan-interrogator`, `gap-check` for planning; `fact-check`, `try-again` for
review; `bite-size`, `plain-english` for communication; and `secret-sweep`,
`shopper`, `loopy`, `ui-redesign-from-reference` as utilities.

## Install

```
claude plugin marketplace add alexschar/claude-skills
claude plugin install alexschar-skills@alexschar-skills
```

## Skills

| Skill | Trigger |
|---|---|
| `spar` | Interviews you into a one-page, verifiable PRD using the SPAR framework. |
| `war-doc` | Wargames a multi-step build move-by-move before code exists. |
| `plan-interrogator` | Rigorously interrogates a plan or PRD before implementation begins. |
| `gap-check` | Hunts for what's absent from a plan, decision, or wrap-up still being decided. |
| `fact-check` | Independent adversarial review of a claim, doc, or another agent's output. |
| `try-again` | Fresh-eyes comparison of what was asked vs. what was delivered, then fixes it. |
| `bite-size` | Breaks updates, plans, or status into small, easy-to-scan pieces. |
| `plain-english` | Jargon-free plain-English translation of session work or a technical concept. |
| `secret-sweep` | Fires when a credential leak or rotation surfaces. |
| `shopper` | Explicit-invocation purchase-research workflow for something you're about to buy. |
| `loopy` | Discover, audit, run, and publish repeatable AI-agent loops. |
| `ui-redesign-from-reference` | Produces a UI/UX redesign plan that maps a visual reference onto an existing codebase. |

Skills that mention `/debate`, `/3rd-party`, `/distill`, `/wrap`, or `/goal`
degrade gracefully if those aren't installed — they're referenced as optional
companions, not hard dependencies.

## License

MIT
