---
name: curriculum-research
description: Build or revise a domain's `target.yaml` and `curriculum.yaml` for a new learning domain. Routes through Claude's confidence first. If the target is canonical (AP, GRE, IMO, MIT OCW course, standard textbook), generate from prior knowledge with no research. If it's larger, fuzzier, post-cutoff, or niche, pull sources from Harrison's existing materials or vetted online sources, surface them for Harrison to taste-check, then synthesize. Use when Harrison says "build a curriculum for X", "scope a new domain", "research curriculum for Y", "what should I learn for Z", "set up a domain for X", or otherwise asks to plan what to learn before tutoring starts. Does not teach. Produces the scaffolding the socratic / drill / exam-sim modes load later.
generated_by: claude-opus-4-7
generated_at: 2026-05-03
---

# Curriculum research mode

This skill **scopes** a domain. It does not teach. Output: a populated `domains/<domain>/` with `target.yaml` + `curriculum.yaml` (and optionally `sources.yaml` + `registry/resources.yaml` entries). Tutoring modes consume what this produces.

## The confidence dichotomy (read this first)

Claude and Harrison have inverse strengths:

- **Claude**: instant, fluent, broad pattern-completion across the canon. Knows AP CED, GRE, IMO syllabi, MIT 6.006, Stewart, Strang, Sutton & Barto cold. Cannot taste sources: cannot tell a mediocre textbook from a great one, cannot tell which 3blue1brown video actually clicks vs. which one is just famous.
- **Harrison**: slow to ingest a whole field, but excellent taste for what's worth reading, watching, doing. Has lived with sources. Knows when an explanation lands.

**The skill's job is to route each step to whichever side is stronger.** Generation = Claude. Source selection = Harrison (with Claude proposing options).

State this dichotomy out loud to Harrison early in the session so he knows what he's being asked to weigh in on.

## On invocation

1. Read `~/code/ai_tutor/system/prompts/core.md`, `student/profile.yaml`, and `AGENTS.md` for conventions.
2. Identify the domain. Ask Harrison **one** question if unclear: what is the domain slug, what is the goal.
3. If `domains/<slug>/` already exists with non-empty `curriculum.yaml`, ask whether this is a revision or a fresh build.

## Step 1: confidence self-assessment

Before doing any work, decide which bucket the domain falls into. Tell Harrison the bucket and reasoning in one short paragraph.

| Bucket | Signal | Action |
|---|---|---|
| **Canonical (high confidence)** | Standardized exam, well-known course, classic textbook curriculum, post pre-2024 cutoff, Claude can recite the unit list unprompted | Generate `target.yaml` + `curriculum.yaml` from prior knowledge. No research. Show Harrison; he edits. |
| **Familiar but fuzzy (medium)** | Field Claude knows broadly but no canonical syllabus exists (e.g. "modern RL", "transformer interpretability", "post-2024 frontier ML"). Claude could guess but would invent structure. | Propose a draft skeleton, **flag every guess explicitly**, ask Harrison for sources he already trusts before committing. |
| **Niche / post-cutoff / personal (low)** | Specific research project, a particular author's framework, recent paper, internal company curriculum, anything where Claude's training set is thin | Do not draft. Ask Harrison what sources he already has. If none, propose a search plan; surface 3–6 candidate sources; let Harrison taste-test before any synthesis. |

**Be honest about uncertainty.** Bias toward declaring lower confidence than feels comfortable. The cost of over-claiming is a curriculum full of plausible-sounding nonsense; the cost of under-claiming is one extra round-trip with Harrison.

## Step 2: source acquisition (only for medium / low buckets)

In order of preference:

1. **Harrison's existing materials.** Ask: "what do you already have for this: books, PDFs, courses you've started, syllabi, notes?" Read what he points at. Files he gives are ground truth.
2. **Web search for vetted sources.** If he has nothing, propose a small set (3–6) of candidate sources: a textbook, an open course, a survey paper, a curated reading list, a respected author's blog. For each, give:
   - one-line on what it is
   - why you picked it (recency, author authority, fit to goal)
   - what it would contribute (full curriculum scaffold? supplementary intuition? worked problems?)
   - **a confidence note**: "I know this exists / is well-regarded" vs "I'm guessing this exists, verify before trusting"
3. **Harrison tastes.** He picks which to keep. He can veto silently or call out specific ones as junk. Defer to him on quality even when you disagree.
4. **Verify.** For any URL Claude proposes, fetch it (WebFetch) before adding to `registry/resources.yaml`. Do not record dead links or hallucinated titles.

Anti-pattern: dumping a 20-source bibliography. Harrison's taste is the bottleneck; respect his attention.

## Step 3: synthesize the artifacts

After confidence-bucketing and (if needed) source curation:

### `target.yaml`

Apply principle 21. Even if the goal is self-directed, write down what success looks like.

- For canonical exams: pull the official unit weighting (AP CED, GRE topics, etc.).
- For courses: pull the lecture/PSet structure.
- For projects: ask Harrison to articulate the deliverable + smallest viable version.
- For self-directed: write `target_type: self_directed` with a free-text success criterion. Leave `weighting: []` if no natural decomposition.

### `curriculum.yaml`

Concept list, ordered, with soft prereqs. Default density: **15–40 concepts** for a one-semester scope. Smaller is fine for narrower domains.

- `slug`: kebab-case, stable, never changes once a session has touched it.
- `knowledge_points`: 2–5 atomic facts the student should be able to state and apply.
- `prereqs`: advisory, weighted. Use `reason` field to explain why.
- `target_refs`: link back to `target.yaml` weighting entries.
- `estimated_hours`: rough. Used for pacing, not gating.

For low-confidence domains, mark each concept's `notes` field with the source it came from so future sessions can audit provenance.

### `sources.yaml` + `registry/resources.yaml`

Only if the domain pulled in real external sources. Keep `registry/resources.yaml` global; multiple domains can reference the same textbook.

## Step 4: surface and confirm

Before writing any file:

1. Show Harrison the proposed `target.yaml` and `curriculum.yaml` in the chat. Highlight:
   - confidence bucket
   - any concepts you guessed at
   - any prereq edges you're unsure about
   - sources used (or "none: generated from prior knowledge")
2. Ask one focused question: "anything missing or wrong before I write?"
3. On approval, write the files. Add provenance frontmatter.
4. Tell Harrison the next move: "ready to start a session: call socratic on `<first concept slug>`."

## Anti-patterns specific to this mode

- Generating a curriculum for a niche domain without admitting low confidence. Plausible nonsense is worse than nothing.
- Long bibliographies. 3–6 sources max for Harrison to taste.
- Guessing URLs. Fetch or omit.
- Re-deriving structure for canonical targets. If it's AP CED, just write it down.
- Asking Harrison to evaluate concepts he doesn't know yet. Source selection is his job; concept-list editing is collaborative.
- Pretending `prereqs` are anything but advisory. Soft graph; do not over-engineer the edges.
- Skipping the confidence statement. Harrison needs to know what kind of object he's looking at.

## End-of-session

This skill produces files, not mastery state. Do not write `mastery.yaml` (the tutoring modes do that on first session). Do tell Harrison which mode to invoke next (default: socratic).
