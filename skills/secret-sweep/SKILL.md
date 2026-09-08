---
name: secret-sweep
description: Use when a credential leak or rotation surfaces — a token, API key, PAT, or password appears in tool output, a log, a commit diff, or a paste; Alex says "rotate the key"; a secret was pushed to any remote; or a rotation item is carried open in a status doc.
---

# Secret Sweep

## Overview

Credential-leak response runbook for Alex's own credentials: contain, scope, rotate, verify, purge, record — in that order, each step gated on the last. Extends security-review (which reviews diffs before they land; this skill handles secrets already exposed) and the standing rule: a read that returns a credential means stop, never requote it, flag for rotation.

## When to Use

- Any tool output, log, commit, or paste that contains a live secret
- Alex asks to rotate a key, or a pushed secret is discovered
- An open rotation item resurfaces in STATUS or memory (e.g. a carried API-key rotation)

## When NOT to Use

- Reviewing code changes for vulnerabilities before merge — that is security-review
- Secrets that are intentionally local and unexposed (Keychain, untracked .env never printed)
- Third-party breach notices with no credential of Alex's exposed

## Quick Reference

Run in order. Do not advance until the current step's gate holds.

| # | Step | Action | Gate |
|---|------|--------|------|
| 1 | CONTAIN | Stop quoting instantly. Never restate the value in chat, files, or memory. Refer to it only by fingerprint: provider + last 4 chars. | Zero further copies exist |
| 2 | SCOPE | Hunt everywhere it traveled: `git log -S` across all branches and remotes, shell history, logs, other tailnet machines. Pushed to GitHub = compromised, regardless of repo visibility. | Written list of every location and consumer |
| 3 | ROTATE | Revoke and reissue at the provider FIRST — before any cleanup. Then update every consumer: env files, launchd plists, Keychain, other machines. | All consumers on the new credential |
| 4 | VERIFY | Prove the old credential dead (an authenticated call fails) AND the new one live at the human-visible surface. A provider "revoked" toast alone is not verification. | Both checks pass, evidence named |
| 5 | PURGE | History rewrite (`git filter-repo`) is destructive and can touch commits Alex did not author — ALWAYS a decision gate for Alex, never automatic. | Alex's explicit yes, or item logged as OPEN |
| 6 | RECORD | Log incident + rotation date in the project status doc; close any carried rotation items. | Status doc updated same session |

Report format (ADHD-friendly): verdict first — "CONTAINED / ROTATED / VERIFIED" or the blocked step — then one bullet per step with its gate evidence. Fingerprint only, never the value.

## Common Mistakes

- Cleaning git history before rotating — the leak stays live the entire cleanup
- Requoting the secret in the incident report, status doc, or memory
- Rotating at the provider but missing a consumer: a launchd plist, a second machine on the tailnet
- Treating a private-repo push as safe — pushed means compromised
- Marking done on the provider's revoke confirmation without proving the old key fails
