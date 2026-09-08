# Per-Project Config — {{PROJECT_NAME}}

```yaml
project: {{PROJECT_NAME}} ({{ONE_LINE_PURPOSE}})
executor_model: {{EXECUTOR_MODEL}}     # wargames each mission, then orchestrates builders
order_depth: 3
wardoc_root: ./wardoc
environment_recon:
  - Read ../INTENT.md and ../CLAUDE.md before anything.
  - {{SOURCE_OF_TRUTH_DOCS — the owner's own written spec / canonical examples}}
  - {{SYSTEMS_TO_MEASURE — the DB, the API, the existing UI; measure with counts}}
target_stack: {{STACK}}
non_negotiables:
  - {{NN_1}}
  - {{NN_2}}
verify_steps:
  - Build runs; preview deploy loads.
  - Defining workflow driven in a real browser; desktop AND phone screenshots.
  - Owner checks the live surface with real data (closes the mission).
```
