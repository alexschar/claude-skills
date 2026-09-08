# WARGAME ORDER (shared — every mission in `tasks/` opens with this)

You are NOT executing this mission yet. You are first WARGAMING it.
The executor is **{{EXECUTOR_MODEL}}** — wargame FIRST: fight the mission on paper move-by-move,
write the result to `wargames/<MISSION_NAME>.md`, THEN execute against that wargame.

## RULES OF ENGAGEMENT
1. **Moves** — Break the mission into discrete moves. For every move state the action (exact
   command / file / step), the **expected observation** (what you see if it worked) and the
   **failure observation** (what you'd see if it didn't).
2. **Counteractions** — every move carries its most likely failure, the cause signals, and the
   counter-move.
3. **Forks** — every decision point gets a trigger: "If you observe X → route A. If Y → route B."
4. **Depth** — simulate consequences to **{{ORDER_DEPTH}}** order.
5. **Assumptions** — any assumption recon can't resolve: flag it and add a `(VARIABLE_NAME)` to
   `LEDGER.md` with a recon lean for the owner's input. Do NOT invent business rules.
6. **Abort conditions** — end each wargame with the exact conditions under which the executor
   must STOP entirely.
7. **Verification** — include a self-verification pass (build runs, flow exercised in a real
   browser, output checked against the canonical example) whose LAST move is the owner's own
   check at the live surface with real data. No "done" without observed evidence.

## STANDING ORDERS FOR THIS PROJECT (do not violate)
- Read `../INTENT.md` first; if any move conflicts with it, STOP and surface it.
- {{NON_NEGOTIABLE_1 — the thing that must survive every block, e.g. "output must match the canonical example exactly"}}
- {{NON_NEGOTIABLE_2 — e.g. "pricing rules are business-critical; ambiguous → (VARIABLE), never a guess"}}
- {{SCOPE_LINE — what is in/out of this phase and the dated ruling behind it}}
- Check `SUCCESS.md` — a wargame is not complete until it satisfies every criterion there.
