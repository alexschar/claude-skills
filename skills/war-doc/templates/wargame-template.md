# WARGAME: {{MISSION}} — {{TITLE}}

> Imports `../_WARGAME_ORDER.md`. Executor: **{{EXECUTOR_MODEL}}** orchestrating scoped builders
> (named-file scope, ≤2 self-QA rounds). Gated by: {{PREREQS}}. Spec written {{DATE}} from the
> owner's typed ask (INTENT-EVIDENCE.md {{DATE}}): "{{VERBATIM ASK}}".

## What this mission is (one paragraph)
{{Plain-language description: what changes for the user, what stays first-class, what it never does.}}

## Recon facts (measured {{DATE}})
- {{fact with a count / exact value / file+line — e.g. "3,234 unit rows; 29% have no usable type"}}
- {{existing seam — the table/column/component the change plugs into}}
- {{what already works that must not regress}}

## Design (decided here; variables called out)
- {{decision and its reason}}
- `(VARIABLE_NAME)` — {{the question}}. Lean: {{recon lean}}. Alternative: {{other route}}.

## Moves
### Move 1 — {{name}}
- **Action:** {{exact command / file / step}}
- **Expected observation:** {{what you see if it worked}}
- **Failure observation:** {{what you'd see if it didn't}}
- **Counteraction:** {{the counter-move and the cause signals}}
- **Fork:** if you observe {{X}} → {{route A}}; if {{Y}} → {{route B}}
- **3rd order:** {{the fallout three layers down if this move silently goes wrong}}
### Move N — Verification + owner's live check
- Self-verification: {{tests; real-browser drive of the defining workflow; 1440 + 390 screenshots into
  `proof/<mission-date>/`; output checked against the canonical example}}
- Owner: {{what they do at the live URL with real data; their confirmation closes the mission}}

## Variables flagged (LEDGER.md)
`(VARIABLE_NAME)` · {{…}} — leans above.

## Abort conditions
- {{missing access / destructive ambiguity / a move that would contradict INTENT.md}}
