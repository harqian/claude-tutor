---
generated_by: claude-opus-4-7
generated_at: 2026-05-06
---

# Card-writing in ai_tutor sessions

**Card design lives in the `mochi` skill** (`skills/mochi/SKILL.md` in this repo, also symlinked to `~/.claude/skills/mochi/` so it loads as a user-level skill anywhere). That is the canonical reference for the six commitments, templates (T1–T10), anti-patterns, source quality, worked examples, and the pre-push self-check.

This file is the ai_tutor-specific layer on top: when to propose during a tutoring session, and how to log what gets pushed.

## When to propose (principle 20)

- Only at learner-flagged insight beats: explicit importance signals, expressed delight, "fundamental" framings, eureka moments. Tutor-perceived "this seems important" does not count — that's how over-carding starts.
- Refuse to card facts the learner did not flag, even if they look card-worthy from the outside.
- Don't break one-step-per-turn flow to draft. Draft silently, surface at the next natural pause.

The pacing ceiling (3–5 cards per hour-long session) and proposal format both live in the mochi skill — apply them as written.

## After approval

- Push immediately via `mcp__mochi__create_flashcard` (no cooling-off — the SRS surfaces bad cards via leech behavior faster than gut-check).
- **Log every card** in the session's `mastery.yaml` log entry under `mochi_cards` (id, deck, front, back). Required whenever cards are pushed — see AGENTS.md. Cards not logged are invisible to future sessions.
- Tag in Mochi by domain and concept slug for cross-reference.

## Note for non-Mochi setups

If the learner uses a different SRS (Anki, etc.), the design canon in the mochi skill still applies — only the push mechanism changes. Substitute the appropriate MCP/CLI for `mcp__mochi__create_flashcard`.
