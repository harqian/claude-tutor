---
generated_by: claude-opus-4-7
generated_at: 2026-05-04
---

# Card-writing guide

Drafting principles and templates for Mochi flashcards proposed during ai_tutor sessions. Loaded by the socratic / drill / exam-sim mode skills at card-draft time. Pairs with principle 20 in `system/prompts/core.md` (when to card); this doc is *how* to card.

The bar: every card pushed to Mochi must be **concise, interest-centered, insightful, engaging, accurate, and corroborated**. If a draft fails any of those, it does not get pushed.

## Section 1 — Six commitments (Harrison's bar, made operational)

1. **Concise and direct.** Answer ≤15 words for the load-bearing fact. Front question is one clause. No "it is important to note," no padding, no half-hearted phrasing. If the back needs a paragraph, the card is wrong — split it.

2. **Centered in interest.** Card content comes from sources Wozniak and Nielsen would recognize: textbooks, primary papers, encyclopedias, Wikipedia. **Never** from corporate blog posts about that company's own product. Vendor-marketing prose is a card-source ban. (Wozniak Rule 20 enumerates books/journals/encyclopedias/dictionaries; blogs are absent. Nielsen: *"if you Ankify misleading work… you're actively making yourself stupider."*)

3. **Insight or breadth, never trivia.** A card earns its place by capturing a non-obvious "aha" OR by compressing a wide territory into a retrievable summary. Symbol-name lookups ("what does ω mean?") and verbatim-quote regurgitation fail this bar.

4. **Engaging prose.** Voice over varnish. The card should read like a sharp human noticed something, not like an encyclopedia summary. *Cut padding, keep the anchor* — one concrete person, scene, or analogy that swaps in for the abstraction. Matuschak: *"the most important thing to optimize is the emotional connection to your review sessions."* Boring cards are diagnostic of bad cards.

5. **Correct and current.** Every claim cross-checked against ≥1 source besides the originating session. Volatile content (software versions, recent research, products) is **date-stamped in the prompt** ("as of 2026-05") or attributed ("Jones 2011 claims…"). Wozniak Rule 19: date your knowledge.

6. **Corroborated.** Settled definitions: 1 authoritative source. Active research: ≥2 independent sources. Product/tool recommendations: ≥3 independent reviews or strong consensus. If you can't corroborate, the card waits.

## Section 2 — The canonical rules (anchor for everything else)

These are the load-bearing rules from the canon. Every draft check below derives from them.

- **Minimum Information Principle (Wozniak Rule 4).** *"The material you learn must be formulated in as simple way as it is possible."* One retrieval target per card. The principle is about per-card simplicity, not per-fact uniqueness — multiple cards on the same fact (different angles) is encouraged.

- **Do not learn if you do not understand (Wozniak Rule 1).** Cards capture insight; insight requires comprehension first. Never card a fact the learner has not actually grasped.

- **Combat interference (Wozniak Rule 11).** *"If knowledge of one item makes it harder to remember another item, we have a case of memory interference."* Wozniak names interference *the single greatest cause of forgetting*. Near-twin cards (chain vs product rule, t vs z, hoop vs sphere) need disambiguating cues baked into the front.

- **Avoid sets and enumerations (Wozniak Rules 9–10).** "Name all members of X" cards fail because the brain enumerates differently each rep, fragmenting the trace. Convert to per-member cards with member-specific cues.

- **Five properties of a good prompt (Matuschak).** Each prompt should be:
  - **Focused** — one retrieval target, no excess detail.
  - **Precise** — vague questions elicit vague answers.
  - **Consistent** — the same prompt produces the same answer every time.
  - **Tractable** — answerable correctly almost every review.
  - **Effortful** — actually requires retrieval, not surface inference.

- **No orphans (Nielsen).** *"5–20 cards or don't bother."* A single isolated card on a topic does not stick. Either build out 2–3 connected cards or drop the singleton.

- **Multiple cues for one fact (Nielsen).** Forward, backward, applied — three angles on the same load-bearing fact, not three reformulations of the front.

- **Generation > recognition (Slamecka & Graf 1978; Roediger & Karpicke 2006).** The learner must produce the answer before flipping. Recognition-only cards forfeit most of the testing-effect benefit.

## Section 3 — Card templates (pattern-match these first)

Use these in roughly this priority order.

### 3.1 Trigger → response (highest leverage for STEM)
**When:** capturing "when problem says X, your move is Y." Most valuable for AP-exam content.
**Front:** *"When you see [situation/feature], what do you reach for?"*
**Back:** named technique or first move, ≤10 words.

> **Example (drafted, AP Stats):**
> Q: When a problem describes "before and after" measurements on the same units, what's the first move?
> A: Compute differences within unit, then 1-sample t on the differences. (Paired, not 2-sample.)

### 3.2 Discriminate near-twins
**When:** two concepts/techniques are easy to confuse (chain vs product rule, paired vs independent, hoop vs sphere). Without these cards, interference produces leeches.
**Front:** *"X vs Y: what's the operational test to tell them apart?"*
**Back:** the discriminating cue (not both definitions).

> **Example (Harrison's existing card, kept):**
> Q: Paired vs independent samples — what's the operational test?
> A: Can you randomly shuffle one group's data without losing information? Yes → independent. No → paired.

### 3.3 Why-this-mechanism (intuition card)
**When:** capturing physical/conceptual intuition behind a result. Back must be a mechanism, not a formula.
**Front:** *"Why does X happen?"* or *"Why is X true mechanistically?"*
**Back:** a causal sentence. If the back is an equation, this template is wrong; use 3.5 instead.

### 3.4 Conditions, decomposed (theorems, hypothesis tests)
**When:** a theorem/test has N conditions. **One card per condition**, plus optionally one count card. Never pack.
**Front:** *"For [theorem] to apply, what condition involves [aspect]?"*
**Back:** the single condition.

### 3.5 Formula with trigger (never formula alone)
**When:** carding a formula. Carding the symbol-string alone is trivia. Pair it with: a trigger card (3.1), a symbol-meaning card per symbol, and a limiting-case sanity card.
**Bad pattern:** Q: "Formula for kinetic energy?" A: "½mv²"
**Good split:** trigger card ("when you need translational KE…"), symbol cards (m, v), limiting-case card ("KE at rest?" → 0).

### 3.6 Multiple cues for one fact (Nielsen)
**When:** a load-bearing claim deserves over-carding. Make 3 cards on the same fact:
- Forward: *"What does Jones 2011 claim about X?"* → "Y."
- Backward: *"Which paper claimed Y about X?"* → "Jones 2011."
- Applied: *"If you cited Y in a paper, who would you cite?"* → "Jones 2011."

### 3.7 Citation/attribution wrapper (uncertain or contested)
**When:** the claim is research-current, contested, or surprising enough that *who said it* matters. Prevents memorizing the claim as ground truth.
**Front:** *"What does [Author Year] claim about X?"*
**Back:** the claim, not as a free-floating fact.

### 3.8 Cloze for ordered structure
**When:** the content has consistent ordering (a formula, a famous quote, a sequence step). Cloze preserves the structural frame and tests the missing token.
**Avoid cloze** for non-ordered content (sets, lists of unordered properties).

> **Example (Nielsen, verbatim, Alan Kay quote):**
> "if the personal computer is truly a {{new medium}} then the use of it would actually change the {{thought patterns}} of an {{entire civilization}}, {{Alan Kay}}, {{1989}}"

### 3.9 Failure-mode card
**When:** the learner just made a specific mistake worth not making twice.
**Front:** *"What's the most common mistake when using X?"*
**Back:** the mistake + the symptom that catches it.

### 3.10 Worked-example "first move"
**When:** a problem type has a characteristic opening move that, once known, unlocks the rest.
**Front:** *"Given a problem with [feature], what's the first move?"*
**Back:** the opening step + which feature triggered it.

## Section 4 — Bad-card patterns to refuse (with fixes)

If a draft pattern-matches one of these, refuse-or-rewrite. Refusing is a positive action; principle 20 demands withholding.

| Anti-pattern | Why it fails | Fix |
|---|---|---|
| **Overlong answer (>15 words)** | Answer becomes a guessing game; partial recall ambiguous | Split into atomic cards; the back is the punchline only |
| **Set / enumeration** ("name all 7 X") | Each rep produces different orders, fragmenting trace | Per-member card with member-specific cue, OR cloze each member in a meaningful frame |
| **Ambiguous prompt** ("What does GRE stand for?") | Multiple correct answers depending on context | Prefix domain context inside the prompt |
| **Yes/no trap** | 50% guess rate; allows pattern-match without retrieval | Convert to open question that demands the same insight |
| **Trivia / encoded-not-understood** | Passes review, no transfer | Pair with trigger card (3.1) so the fact has a deployment context |
| **Orphan card** | One isolated fact, no neighborhood | Either build 2–3 connected cards or drop |
| **Cards from inside the book** | Depends on book's framing/example | Rewrite so a stranger could answer |
| **Verbatim recitation** | Memorizes phrasing, not concept | Card the underlying idea; if quote matters, cloze the load-bearing token |
| **Symbol-name lookup** ("What is ω?") | Trivia, no deployment cue | Replace with trigger ("When do you need ω?") + meaning card |
| **Compound back ("…and…")** | Two retrieval targets disguised as one | Split at the "and"; this is the single most common atomicity failure |
| **Numerical answer** ("What's the speed?") | The number is incidental; the *setup choice* is what matters | Card the setup decision and the failure mode, never the number |
| **AI-slop encyclopedia tone** | Generic textbook voice, no anchor | Rewrite in tutor voice with one concrete hook |

## Section 5 — Source quality (criterion 2 + 6, made concrete)

**Decision tree, in order:**

1. **Frontier topic (active research):** primary paper(s) + a survey/review article. ≥2 independent sources before carding.
2. **Stable canonical (textbook content, AP-curriculum, classical results):** textbook ≥ Wikipedia ≥ class notes. Cross-check ≥1 second source.
3. **Tool / software interface fact:** official spec, version-tagged in the prompt. *Single source acceptable* but date-stamp.
4. **Tool best-practice or product recommendation:** ≥3 independent reviews + spec. **Vendor blog about own product is a hard ban.**
5. **Vendor blog as discovery surface:** OK to *find* a topic, never the citation. Trace to primary.
6. **Volatile content (versions, prices, current research, recent events):** date-stamp in the prompt body, not just metadata.

**Wikipedia-tier sourcing as Wozniak frames it (verbatim):** *"the recommended source of basic incremental reading materials"* — encyclopedic prose where *"each sentence has a contribution to your knowledge."*

## Section 6 — In-session workflow (principle 20, expanded)

**When to propose:**
- Only at learner-flagged insight beats: explicit importance signals, expressed delight, "fundamental" framings, eureka moments. Tutor-perceived "this seems important" does not count — that's how over-carding starts.
- Refuse to card facts the learner did not flag, even if they look card-worthy from the outside.

**Pacing:**
- Working ceiling **3–5 cards per hour-long session**. 0 is fine for grind sessions where no insight surfaced. Quantum Country's 27/hr rate is for dense reading, not tutoring.
- Don't break one-step-per-turn flow to draft. Draft silently, surface at the next natural pause.

**Proposal format (in chat):**

```
Card draft (insight: <what triggered it>)

  deck: <domain>
  tags: <concept-slug>
  front: <prompt — prefer learner's verbatim phrasing>
  back:  <atomic, one fact, ≤15 words>

Approve / edit / skip?
```

**Refuse with a named rule.** Never just "no." If declining: *"This is a Rule 1 case — not yet understood,"* or *"Orphan card — only this concept; want me to draft 2 more siblings?"* or *"Trivia trap — symbol lookup with no trigger; want a trigger card instead?"*

**Push immediately on approval.** No cooling-off. The SRS surfaces bad cards itself via leech behavior; that's faster than gut-check.

**Log every card** in the session's `mastery.yaml` log entry under `mochi_cards` (id, deck, front, back). Required whenever cards are pushed — see AGENTS.md. Cards not logged are invisible to future sessions.

## Section 7 — STEM-specific formatting (Mochi + KaTeX)

- **Mochi cloze syntax is `{{...}}`, not Anki's `{{c1::...}}`.** Numbered groups: `{{1::...}}` `{{2::...}}` reveal together.
- **Math:** inline `$x$`, block `$$...$$`. KaTeX renders most LaTeX; `gather*`, `multline`, and `\newcommand` per-card don't work — use Mochi's global LaTeX macros for repeated definitions (`\hat{p}`, `\E`, `\Var`).
- **Mix prose with math.** Reserve `$$...$$` for the equation that *is* the answer. Cards wrapped entirely in display math with `\text{}` English are the canonical anti-pattern.
- **Match the textbook's notation exactly** — if AP uses `\vec{v}`, use that, not `\mathbf{v}`. Retrieval is surface-form sensitive; don't invent house style.
- **Symbol prompts use the symbol, not the phonetic name.** "What is $\omega$?" not "What is omega?" — and pair with a meaning card.
- **Units are part of correctness for physical quantities**, not for dimensionless ones or symbolic identities. Format with `\,\mathrm{...}`.
- **Mochi gotchas:** stray `$` triggers math mode (escape `\$`); blank lines inside `$$` break the block; markdown does not render inside HTML tags.
- **Algorithmic note:** Mochi is binary (Forgot / Remembered). "Mostly remembered" = Forgot. This punishes compound cards harder than Anki's 4-button system. Atomicity is enforced by the algorithm, not just principle.

## Section 8 — Worked examples (verbatim from canon + drafted STEM)

### Verbatim from the canon (lift these as-is)

**Wozniak — Dead Sea (the canonical Minimum Information Principle demo):**

BAD:
> Q: What is the Dead Sea?
> A: A salt lake located on the border between Israel and Jordan. Its surface and shores are 423 metres below sea level, Earth's lowest elevation on land. It is 377 m deep, the deepest hypersaline lake in the world. […]

GOOD (atomic split):
> Q: Where is the Dead Sea located? / A: on the border between Israel and Jordan
> Q: What is the lowest point on Earth's land? / A: shores of the Dead Sea (423 m below sea level)
> Q: How deep is the Dead Sea? / A: 377 m

**Matuschak — chicken stock (avoiding binary prompts):**

BAD: *"Does chicken stock make vegetable dishes taste like chicken?"* (yes/no trap)
GOOD: *"How does chicken stock affect the flavor of vegetable dishes?"* / *"It makes them taste more 'complete'."*

**Matuschak — omelette (eliminating ambiguity):**

BAD: *"What's the first step to cook an omelette?"* (multiple defensible answers)
GOOD: *"When making an omelette, how must the pan be prepared before you add the eggs?"*

**Nielsen — soft link (atomicity by failure-dimension split):**

BAD: *"How do you create a Unix soft link?"* (two retrieval targets: command and arg order)
GOOD:
> Q: What's the basic command and option to create a Unix soft link? / A: `ln -s …`
> Q: When creating a Unix soft link, in what order do `linkname` and `filename` go? / A: `filename linkname`

### Drafted STEM examples

**Trigger + atomicity (AP Physics, rolling):**

BAD (Harrison's current rolling card — fused):
> Q: Two solid spheres of different mass and radius roll without slipping down the same incline from the same height. Which arrives at the bottom faster?
> A: Same speed. v = √(2gh / (1 + I/(mR²))). For any solid sphere I/(mR²) = 2/5 regardless of m or R. Mass and radius cancel; only shape and h matter.

GOOD (split):
> Q: Two solid spheres, different m and R, same incline, same h, rolling without slipping — which wins? / A: Tie.
> Q: Why doesn't mass or radius affect rolling-without-slipping speed for the same shape? / A: $v = \sqrt{2gh / (1 + I/(mR^2))}$ depends only on shape; for a solid sphere $I/(mR^2) = 2/5$ regardless of m, R.
> Q: For rolling without slipping, what dimensionless quantity decides who wins down an incline? / A: $I/(mR^2)$ — smaller wins.

**Conditions decomposed (AP Stats, 2-sample t):**

BAD: *"What are the conditions for the 2-sample t-test?"* (set card)
GOOD (one card per condition):
> Q: For 2-sample t, what condition involves how the data was collected? / A: Independent random samples (or random assignment for an experiment).
> Q: For 2-sample t, what condition involves the two samples' relationship? / A: Independence between samples.
> Q: For 2-sample t, what condition involves shape? / A: Each sample roughly normal OR n large enough for CLT.

## Section 9 — When NOT to make a card

Refuse, with a named rule, when:

- The learner has not flagged the moment as important (principle 20 violation).
- The concept is not yet understood (Wozniak Rule 1).
- The fact is a single orphan with no neighborhood and no plan to build one.
- The fact is procedural skill, not declarative knowledge — drilling produces the fluency, cards don't.
- The content is verbatim quotation from a book, with no underlying concept to lift.
- The learner is in a grind/rep session where flow > capture.

The cost of a refused card is zero. The cost of a slop card pushed to Mochi is reviewing it for years.

## Section 10 — Self-check before pushing

Run this checklist on every drafted card. If any fails, revise or refuse.

- [ ] One retrieval target on the back? (Atomicity)
- [ ] Back ≤15 words for the load-bearing fact?
- [ ] Front uniquely cues this answer (no twin would also be correct)?
- [ ] Front is precise — would I write the same answer 6 months from now?
- [ ] If a formula: paired with a trigger card and a symbol card?
- [ ] Source is canonical (textbook / paper / Wikipedia / authority), not vendor-marketing?
- [ ] If volatile: date-stamped in prompt?
- [ ] If contested: attribution-wrapped ("X claims Y")?
- [ ] At least 2 sibling cards in the same neighborhood (no orphan)?
- [ ] Voice is sharp tutor, not encyclopedia summary?
- [ ] Did the learner flag this moment? (Principle 20)

If all pass: push, log to `mastery.yaml`, move on.

## References

Primary canon:
- Wozniak (1999/updated), *Effective learning: Twenty rules of formulating knowledge.* super-memory.com/articles/20rules.htm
- Wozniak, *Minimum Information Principle.* supermemo.com / supermemo.guru
- Matuschak, *How to write good prompts.* andymatuschak.org/prompts
- Nielsen (2018), *Augmenting Long-term Memory.* augmentingcognition.com/ltm.html
- Nielsen, *Using spaced repetition systems to see through a piece of mathematics.*
- Matuschak & Nielsen, *How can we develop transformative tools for thought?* numinous.productions/ttft

Cognitive psych anchors:
- Roediger & Karpicke (2006). The testing effect. *Psychological Science* 17, 249–255.
- Slamecka & Graf (1978). The generation effect. *JEP:HLM* 4, 592–604.
- Craik & Lockhart (1972). Levels of processing. *JVLVB* 11, 671–684.
- Rogers, Kuiper & Kirker (1977). Self-reference effect. *JPSP* 35, 677–688.
- Bjork & Bjork (2011). Desirable difficulties. UCLA chapter.
- Rohrer & Taylor (2007). Interleaving. *Instructional Science* 35, 481–498.

Research dossier (background for this doc):
- /Users/hq/Downloads/flashcards-research/MASTER.md (synthesis index)
