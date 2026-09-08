---
name: plain-english
description: Use when Alex says "/plain-english", "explain that simply", "no jargon", "in plain English", "explain it like I'm not technical", "simple breakdown", "what does that actually mean", or asks what's happening with her work "in normal words". Applies to explaining current session work, a project's status, a technical concept, an error, or a document she's looking at.
---

# /plain-english — jargon-free translation

Purpose: translate whatever is being discussed into language a smart non-technical
person gets on first read. A translation layer, not a shorter summary — the record
(blockers, open items, decisions) still shows up, just renamed into plain words.

This complements, not replaces, the length/brevity rules in the user's CLAUDE.md
Communication modes. It extends nothing else — no existing skill governs jargon
level or audience translation, which is why this one exists.

## Output shape

**"What's happening with my work" requests** (session status, project status):
1. What you asked for
2. What I'm doing and why
3. What's done — state the proof in plain terms
4. What's NOT done yet
5. What happens next

**Concept / error / document explanations:**
1. What it is
2. Why it matters to Alex
3. What (if anything) she needs to do

## Hard rules

- No file paths, no tool/agent/model names, no code identifiers.
- No acronym without a 3-word gloss on first use.
- Everyday analogies are fine ("mystery shopper" for a beta tester) — only if accurate.
- Keep numbers concrete ("six serious problems", "$25K proposal") — specifics build trust, vagueness doesn't.
- Status words are honest, not hedged: "done", "half-done", "not started", "broken" — never "partially implemented" or similar softening.

## Length

Lead with the answer. Whole response under ~250 words unless Alex asks for more.
Bold the handful of phrases that actually carry the meaning — not every noun.

## Quick check before sending

- Would this make sense to someone who has never used this tool or written code?
- Did every blocker/open item/decision from the real record survive the translation?
- Any stray file path, model name, or acronym slipped in?
