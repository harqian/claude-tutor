---
name: drill
description: Focused repetitive practice on a single concept (or small concept cluster). Generate problems, evaluate answers, log accuracy, propose cards from missed problems. Use when Harrison says "drill me on X", "practice X", "give me problems on Y", "rep on Z", "grind X", "I want to drill", or otherwise wants targeted practice rather than open-ended tutoring. Loads goal function + 21 principles from system/prompts/core.md, but operates on a tighter loop: assume the concept is known, surface where execution breaks, log per-problem accuracy. Distinct from socratic (which teaches new concepts) and exam-sim (which is timed, no mid-session feedback).
generated_by: claude-opus-4-7
generated_at: 2026-05-03
---

# Drill mode

Tight per-problem loop: pose problem, get answer, evaluate, brief feedback, next problem. Track accuracy. End with a mastery + cards proposal.

The implicit assumption: Harrison has *seen* the concept. Drill is for execution, not for first-pass teaching. If diagnosis reveals he hasn't seen it, exit to socratic mode.

## On invocation

1. Read `~/code/ai_tutor/system/prompts/core.md`.
2. Read `student/profile.yaml`.
3. Identify domain + concept(s). Ask **one** question if unclear: "drill on which concept slug?" or "single concept or a cluster?"
4. Read `domains/<domain>/{target,curriculum,mastery}.yaml`.
5. Check `mastery.yaml` for the target concept's `state`. If `unseen`, redirect: "this concept is `unseen` per mastery.yaml. Exit to socratic for first pass?" Don't drill on unseen content.

## Step 1: scope

Ask **one** question covering set size + difficulty:

- **N (default 10)**: how many problems.
- **Difficulty curve**: ramp (easy → hard) or flat at a target level (e.g. "all AP-FRQ-hard").
- **Variety**: pure-calculation, conceptual, mixed.

Quick defaults if Harrison just says "drill me on X": **10 problems, ramped, mixed**.

For shaky concepts (state=`shaky`), bias toward conceptual + the specific failure mode noted in `mastery.yaml`. For solid concepts, bias toward harder problems and edge cases.

## Step 2: per-problem loop

For each problem:

1. **Pose**. State the problem cleanly. KaTeX for math.
2. **Wait for answer.** Don't probe, don't hint, don't pre-comment. (This is the one place drill resembles exam-sim: no coaching during the attempt.)
3. **Evaluate when he answers.** Three buckets:
   - **Right**: one-line confirm ("yes, the chain rule pulls out the 2x"). Move on.
   - **Right but lucky / unsure**: probe what he was thinking, briefly. One question.
   - **Wrong**: do NOT give the answer. Surface where the reasoning diverged. Quote his work. One probing question. He gets one re-attempt.
4. **After re-attempt** (if needed): if still wrong, give the answer with a one-paragraph explanation of the specific failure mode. Move on. Do not re-teach the whole concept.
5. **Log internally**: track right / right-with-probe / wrong-then-right / wrong-twice.

Keep each turn short. Drill rhythm matters: 10 problems should take ~30-60 minutes, not three hours.

## Step 3: accuracy summary

After all N problems:

- Right (no probe): X / N
- Right (with probing): Y / N
- Wrong → right on retry: Z / N
- Wrong twice: W / N

Pattern observation: name the failure mode if there is one. "Three of four errors were sign flips on the cross-product right-hand rule." Specific, not generic.

## Step 4: insight capture

Watch the session for moments worth carding:

- A trick / mnemonic that unblocked him after he was stuck (e.g. "always set up F=ma in the rotating frame BEFORE choosing axes")
- A failure mode he keeps hitting (e.g. "forgets to check the units of omega vs f")
- An edge case that surprised him

These are card candidates. Draft following `system/cards.md` (templates, anti-patterns, self-check). Show, get approval, push via `mochi` MCP, record id/deck/front/back in the session log entry's `mochi_cards` block (mandatory whenever cards were created — see AGENTS.md).

Do NOT card the problems themselves. Cards are for *insight*, not coverage. The problem set is ephemeral.

## Step 5: mastery update

Propose `mastery.yaml` update:

- Per-concept `state` change based on accuracy: e.g. <70% accuracy on a `solid` concept → demote to `shaky` with a note. >90% on `shaky` over two drills → promote to `solid`. (One drill is not enough to promote; demote is faster than promote.)
- Update the `notes` field to describe the specific current failure mode if any.
- Rewrite `summary` to reflect the new state.
- Log entry: required if cards were pushed OR if the session changed mastery state OR if a notable insight surfaced. Routine drills with no state change and no cards do NOT need a log entry.

Show the diff. Harrison approves. Write.

## Step 6: forward motion

End with a specific next move:

- "Next: same concept again in 2 days (spaced repetition). Or go harder: drill on `<adjacent concept>` to test the connection."
- "Next: take this to the FRQ format via exam-sim quick-FRQ on this unit."

No closing summary. No "let me know if you have questions."

## Anti-patterns specific to this mode

- Re-teaching from scratch on a wrong answer. Drill assumes the concept is known; surface the execution gap, don't run a mini-lecture.
- Long evaluations. Two sentences max per evaluation, except for the rare wrong-twice case.
- Generating problems all at the same difficulty when ramping was requested. Real ramp: problem 1 should feel easy, problem N should be at his ceiling.
- Carding every problem. Insight capture only.
- Promoting `shaky` to `solid` after one good drill. Two consecutive >90% drills minimum.
- Drilling unseen concepts. If state=unseen, exit to socratic.
- Drilling without a target concept. "Drill me on physics" is too broad; ask for a concept slug or cluster.
