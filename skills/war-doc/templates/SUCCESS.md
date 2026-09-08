# Wargame Success Criteria

A wargame file (`wargames/<mission>.md`) is COMPLETE only when:
- [ ] Opens with measured **Recon facts** (counts, distinct values, existing seams) — not assumptions
- [ ] Every move has an **expected observation** AND a **failure observation**
- [ ] Every move has its most-likely failure, the cause signals, and a counter-move
- [ ] Every fork has an explicit "if you observe X → route Y" trigger
- [ ] Consequences are simulated to the configured order depth
- [ ] All unresolved assumptions are `(VARIABLES)` in `LEDGER.md`, each with a recon lean
- [ ] **Abort conditions** are listed
- [ ] A **self-verification pass** is defined whose last move is the owner's live check with real data
- [ ] It respects every Standing Order in `_WARGAME_ORDER.md` and never contradicts `../INTENT.md`

## Project-level "done" for {{PROJECT_NAME}}
- [ ] {{DONE_1 — observable by the owner at the live surface}}
- [ ] {{DONE_2}}
