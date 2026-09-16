# alexschar-skills

A Claude Code plugin packaging 16 self-contained skills for planning, review,
communication, design & explainer work, and everyday utility work: `spar`,
`war-doc`, `plan-interrogator`, `gap-check` for planning; `fact-check`,
`try-again` for review; `bite-size`, `plain-english` for communication;
`instruction-artifact`, `design-taste-recall`, `design-dna`, `office-guide`
for design & explainer work; and `secret-sweep`, `shopper`, `loopy`,
`ui-redesign-from-reference` as utilities.

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

### Design & explainer

| Skill | Trigger |
|---|---|
| `instruction-artifact` | Builds a self-contained, animated HTML tutorial for a non-technical beginner, packaged to deliver. |
| `design-taste-recall` | Pulls documented visual taste before any design work, and captures reactions after. |
| `design-dna` | Extracts a reference site's look into a reusable DESIGN_BLUEPRINT.md. |
| `office-guide` | Builds a calm, print-first office how-to with numbered steps, screenshots, and copy blocks. |

Skills that mention `/debate`, `/3rd-party`, `/distill`, `/wrap`, or `/goal`
degrade gracefully if those aren't installed — they're referenced as optional
companions, not hard dependencies.

## Design knowledge folder

`design-taste-recall` and `design-dna` read from an optional `.claude/design-taste/`
folder in your home directory (Windows: `%USERPROFILE%\.claude\design-taste\`).
When present, it holds:

- `DESIGN_TASTE.md` — a curated distillation of your visual taste, with per-surface
  modes and pass/fail bars.
- `DESIGN_TASTE_EVIDENCE.md` — dated, verbatim reactions to past visual work; the
  tiebreaker when modes disagree.
- `DESIGN_BRIEF_RULES.md` — how to write visual briefs for builders.
- `examples/` — reference images named by mode.
- `blueprints/` — extracted `DESIGN_BLUEPRINT.md` files from past reference sites.

The plugin works without this folder — both skills fall back to inline
non-negotiables and pass/fail bars — but recall gets sharper the more of it
exists.

## License

MIT
