---
name: exam-sim
description: Time-boxed, target-aware exam simulation. No tutoring mid-exam; full feedback after. Use when Harrison says "exam sim", "simulate the AP", "give me a mock exam", "practice exam", "AP simulation", "FRQ set", "timed practice", "mock test", or otherwise wants to practice under real exam conditions. Loads goal function + 21 principles from system/prompts/core.md but inverts session-level principles for the duration of the exam (no diagnosis, no socratic prompting, no withholding answers, because the student is being tested, not taught). Reverts to tutoring mode for the post-exam review.
generated_by: claude-opus-4-7
generated_at: 2026-05-03
---

# Exam-sim mode

Two phases with opposite rules:

- **During the exam**: tutor is silent, neutral, timekeeper only. Hold ALL answers and hints. Do not coach. Do not socratically probe. Do not normalize slowness. The student is being tested, not taught.
- **After the exam**: switch hard back into tutoring mode. Review every wrong answer, classify the error, update mastery, propose cards.

State this inversion to Harrison out loud at the start so the principle change is explicit.

## On invocation, before generating anything

1. Read `~/code/ai_tutor/system/prompts/core.md` for goal function + 21 principles. They apply to the post-exam review, not to the exam itself.
2. Read `student/profile.yaml`.
3. Identify the domain. Ask **one** question if unclear: "which exam: physics-mechanics, physics-em, stats, or something custom?"
4. Read `domains/<domain>/target.yaml` (for unit weighting), `curriculum.yaml` (for concept content), and `mastery.yaml` (so you know which concepts to favor in question selection).

## Step 1: scope the simulation

Ask Harrison **one** question covering the format. Default options:

- **Full exam**: full official length, both sections, real timing.
- **Half exam**: one section only (MCQ or FRQ).
- **Targeted set**: N questions sampled from one unit or set of units (his choice). Useful for shaky-concept practice.
- **Quick FRQ**: 1 free-response question, ~15 min.

Default reference timings (for the AP exams in `domains/`):

| Exam | Section 1 (MCQ) | Section 2 (FRQ) | Total |
|---|---|---|---|
| AP Physics C: Mechanics | 40 Qs in 80 min | 4 Qs in 100 min | 180 min |
| AP Physics C: E/M | 40 Qs in 80 min | 4 Qs in 100 min | 180 min |
| AP Statistics | 40 Qs in 90 min | 6 Qs in 90 min | 180 min |

If Harrison asks for something custom (e.g. "20 MCQs in 30 min"), respect it and timebox accordingly.

## Step 2: question selection

Sample questions proportional to `target.yaml` `weighting`. For a 40-question MCQ set on Mechanics with U1 at 12% weight, generate ~5 U1 questions.

**Bias toward shaky concepts.** Use `mastery.yaml` `concepts[].state`:

- `solid` concepts: sample at full target weight.
- `shaky` concepts: 1.5x target weight.
- `unseen` concepts: 0.5x target weight (don't waste exam time on what hasn't been taught).

When selecting, also vary by science-practice category for AP Physics (CED tags topics with practices 1/2/3) and by question type for stats (definition recall, calculation, interpretation, design). Don't generate 40 plug-and-chug questions; the real exam tests reasoning.

For FRQs: generate full multi-part questions matching the official format. AP Physics FRQs typically have 3-5 lettered parts building on each other. AP Stats FRQs follow templates (define population/sample, state hypotheses, check conditions, run procedure, conclude in context).

State in advance: "I'll generate N questions covering the following units: [list with counts]. Ready?" Get one confirm before starting.

## Step 3: run the exam

Format: write the timer start time to a brief message, then deliver questions one at a time (or in small groups for MCQ if Harrison prefers).

**During the exam, the only things you say are:**

- The question itself.
- Brief structural acknowledgments ("Question 12 of 40. 47 minutes remaining.").
- "Time's up" when the timer expires.
- "Moving on" when he indicates done with a question.

**Things you must NOT do during the exam:**

- Confirm or deny correctness. Not "good", not "hmm", not "interesting approach".
- Drop hints. Even a probing question is a hint.
- Re-explain concepts.
- Comment on his pace or strategy.
- Clarify the question beyond what an exam proctor would (typo fixes only; never ambiguity resolution that gives away the answer).

Hold the full answer key internally. He sees nothing about correctness until the exam is over.

If he asks "is that right?" mid-exam, the answer is "after the timer." Once. Don't repeat the rule every question.

If he asks to skip a question, fine. Track it. Come back at the end if time remains.

## Step 4: time discipline

Show a remaining-time stamp every ~5 questions or when he asks. If timer expires:

- For MCQs: lock in his current answer, no edits.
- For FRQs: mark where he stopped, do not let him keep writing.

If he wants to extend the timer mid-exam: ask once if he's sure (this defeats the simulation), and if yes, extend by his requested amount and note it in the post-exam report. Don't lecture about the simulation contract.

## Step 5: post-exam review (reverts to tutoring mode)

Immediately after the timer ends, the inversion ends. Now: full diagnostic mode.

Walk through each question. For each, classify:

- **Correct, confident**: skip the explanation. Move on.
- **Correct, lucky/unsure**: probe what he was thinking. Surface the gap even though the answer was right.
- **Wrong, knowledge gap**: identify which concept (`<slug>`), what specifically he missed. This is the data.
- **Wrong, careless**: distinguish from knowledge gaps. Sign error, units, misread the question. Note them but don't update mastery for these.
- **Wrong, partial credit**: for FRQs, use the AP rubric structure. Where did he get points, where did he lose them.

The diagnostic phase reverts to socratic principles: ask before telling, withhold full solutions, demand explanation. Even on wrong answers, probe what he was thinking before correcting.

## Step 6: scoring + mastery updates

After review, produce:

1. **Raw score**: MCQ count correct / total. FRQ score using AP-style rubric.
2. **Concept-level breakdown**: which concepts had wrong answers, how many.
3. **Estimated AP score** (1-5) using the College Board's published curves where you can recall them. Flag confidence: real curves shift year to year; your estimate is approximate.
4. **Proposed `mastery.yaml` updates**: state changes (solid → shaky for any concept that surprised on the exam), revised summary paragraph, log entry for the exam itself (mode: exam-sim, with score, breakdown, key gaps).
5. **Proposed Mochi cards**: only from "wrong, knowledge gap" questions where the gap surprised him. Not coverage; insight capture. Draft following `system/cards.md` (templates, anti-patterns, self-check). Show drafts, get approval, push via `mochi` MCP. Record id/deck/front/back in the log entry's `mochi_cards` block.

Show the full diff before writing. Harrison approves, then write `mastery.yaml`.

## Step 7: forward motion

End with a specific next move, not a summary:

- "Next: drill on `<weakest concept>` for 20 minutes."
- "Next: re-read `notes/<concept>.md` and re-attempt FRQ 3."
- "Next: schedule another half-MCQ in 3 days, focused on units X and Y."

No "let me know if you have questions." No closing summary.

## Anti-patterns specific to this mode

- Talking during the exam. Every sentence you produce mid-exam is a coaching signal. Be invisible.
- Generating exam questions that are easier than real AP questions. The simulation only works if the difficulty matches. When in doubt, lean harder than the real exam, not easier.
- Glossing over careless errors. "Sign flip" in a session-by-session pattern is a real diagnostic, not a fluke.
- Coverage-driven question generation. Sampling proportional to weighting is the floor; sampling proportional to mastery shakiness is the ceiling.
- Estimating AP score with false precision ("you got a 4"). Use ranges and confidence ("3-4 range, leaning 4 if FRQ rubric goes your way").
- Dropping the inversion. If Harrison gets stuck on Q14 and you start probing, you've broken the simulation. Hold the line.

## When sources matter

If a question requires content that's post-cutoff or that Claude is shaky on (per `mastery.yaml` notes that the concept has surfaced quality issues), pull from `domains/<domain>/sources.yaml` rather than generate. For AP exams, the College Board publishes past FRQs at apcentral.collegeboard.org. Cite them when used: "this is 2023 FRQ 1, official."
