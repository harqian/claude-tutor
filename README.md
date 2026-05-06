# ai_tutor

Harrison's personal AI-tutor system. Markdown + folders + Claude skills + Mochi for spaced repetition. The whole repo is the LLM's environment, not a prompt block.

Operational rules live in [`AGENTS.md`](AGENTS.md). Read that before changing structure.

## Goal function

The tutor's goal is **to leave the learner more capable**, not to resolve the learner's question. When those goals diverge, the second one wins. Every other rule is a corollary. This single line overrides RLHF "helpfulness" pressure that defaults to fast complete answers.

## Layout

```
~/code/ai_tutor/
├── AGENTS.md                       # project rules (the spec)
├── CLAUDE.md                       # @AGENTS.md
├── README.md                       # this file
├── .claude-plugin/                 # packages the repo as a Claude Code plugin
│   ├── plugin.json                 # plugin manifest (name: ai_tutor)
│   └── marketplace.json            # local marketplace so settings can enable it
├── .claude/settings.json           # enables the ai_tutor plugin when cwd is this repo
├── skills/                         # mode skills, surfaced as ai_tutor:<name>
│   ├── socratic/SKILL.md
│   └── curriculum-research/SKILL.md
├── system/
│   ├── prompts/core.md             # goal function + 21 principles, loaded by every mode
│   └── visualization.md            # viz playbook (Ciechanowski-grade craft)
├── registry/
│   └── resources.yaml              # global resource registry (textbooks, OCW, videos)
├── student/
│   ├── profile.yaml                # who Harrison is, learning preferences
│   └── learning-roadmap.md         # canonical meta-learning doc (Harrison-authored)
├── domains/
│   └── <domain_slug>/
│       ├── target.yaml             # exam / project / deliverable structure (or null)
│       ├── curriculum.yaml         # ordered concept list with soft prereqs
│       ├── sources.yaml            # which resources for this domain
│       ├── mastery.yaml            # living mastery state + selective log
│       ├── notes/<concept_slug>.md # flat folder of LLM-authored study notes
│       ├── scratch/                # Harrison's own writing (no frontmatter, untouched by tutor)
│       └── problems/               # exercises
└── out/artifacts/                  # generated viz, dated, never overwritten
```

Skills are packaged as a Claude Code **plugin** named `ai_tutor`. They appear in the slash-command list namespaced (`ai_tutor:socratic`, `ai_tutor:curriculum-research`) so all available modes are visible at a glance. Edit `skills/<name>/SKILL.md` directly; run `/reload-plugins` to pick up changes.

## Skills

| Skill | Trigger | Purpose |
|---|---|---|
| `socratic` | "tutor me on X", "teach me Y" | Default tutoring. Diagnose-first, withhold-answer, one-step pacing. |
| `curriculum-research` | "build a curriculum for X", "scope a new domain" | Build `target.yaml` + `curriculum.yaml` for a new domain. Routes by Claude's confidence: canonical → generate from memory; niche / post-cutoff → pull sources from Harrison or web, surface for him to taste-test. Does not teach. |
| `drill` | "drill me on X", "practice X" | Focused per-problem practice on a single concept. Track accuracy, surface failure modes, propose cards from insight moments. Assumes the concept is known. |
| `exam-sim` | "exam sim", "AP simulation" | Time-boxed, target-aware, no coaching mid-exam. Inverts socratic principles during the exam, reverts for the post-exam review. |
| `compiler` *(planned)* | "code with me on X" | Natural-language-compiler for code. Refuse to write underspecified code. |
| `explain` *(planned)* | "explain X" | Concession mode: when student needs a direct answer. The off-switch for socratic. |

Every mode skill loads `system/prompts/core.md` (goal function + 21 principles) on top of its own behavior.

### Confidence-driven curriculum building

Claude and Harrison have inverse strengths: Claude has fast, fluent pattern-completion across the canon (AP CED, GRE, MIT 6.006, classic textbooks) but no taste for source quality. Harrison has lived with sources and knows what lands. The `curriculum-research` skill routes generation to Claude and source selection to Harrison explicitly, with Claude declaring its confidence bucket up front (canonical / familiar-but-fuzzy / niche-or-post-cutoff) before doing any work. See [`skills/curriculum-research/SKILL.md`](skills/curriculum-research/SKILL.md).

## Principles

The 21 principles (10 from the AI-tutoring literature, 11 from Harrison) live in [`system/prompts/core.md`](system/prompts/core.md). All session behavior derives from there. Highlights:

- **Diagnose before teaching.** "Do you understand?" doesn't count.
- **Never state the answer.** Hold under "just tell me" pressure.
- **One step, one question, one chunk per turn.**
- **Student talks more than tutor.**
- **Treat wrong answers as data**, not failures to re-explain.
- **Mastery as checked-in artifact.** `mastery.yaml` per domain, updated each session.
- **LLM-first, escalate to source curation only on quality failure.**
- **Flashcards as insight capture, not coverage.**
- **Elicit target structure** before any session.

## Schemas

See [`AGENTS.md`](AGENTS.md) for conventions and `domains/<domain>/` files for live examples once a domain exists. Key shapes:

- `student/profile.yaml`: calibration (cognitive style, modes, tone).
- `domains/<domain>/target.yaml`: `target_type` (`ap_exam` | `mit_ocw_course` | `research_project` | `self_directed` | `null`), `weighting`, `deadline`.
- `domains/<domain>/curriculum.yaml`: `concepts[]` with `slug`, `knowledge_points`, soft `prereqs`, `target_refs`, `estimated_hours`.
- `domains/<domain>/mastery.yaml`: `summary` (rewritten each session), `concepts[].state` (`unseen` / `shaky` / `solid`), selective `log[]` with mandatory `mochi_cards` block whenever cards were created.
- `domains/<domain>/sources.yaml` + `registry/resources.yaml`: concept→resource bindings + global resource registry.
- Note files: frontmatter (`slug`, `domain`, `tags`, `prereqs`, `generated_by`, `generated_at`, `last_session`) + KaTeX-friendly markdown.

All LLM-authored files carry `generated_by` + `generated_at`. No human-review flag.

## Visualizations

Self-contained HTML to `out/artifacts/YYYY-MM-DD_HHMMSS_<slug>.html`. Never overwrite. CDN imports, inline JS. `live-server` runs in `out/`; tell Harrison `localhost:5500/artifacts/<filename>`.

Library defaults and the 15 craft principles (Ciechanowski-grade bar) live in [`system/visualization.md`](system/visualization.md). Read it before generating viz.

One-time setup:

```bash
npm install -g live-server
mkdir -p ~/code/ai_tutor/out/artifacts
cd ~/code/ai_tutor/out && live-server --port=5500 --no-browser &
```

## Architecture decisions (locked, v0)

| Decision | Choice |
|---|---|
| Mastery state | `domains/<domain>/mastery.yaml`, living, updated each session, log embedded. |
| Vault scope | Mega-vault. Domains separated by tags + folder. |
| Concept granularity | NO per-concept folders. Flat `notes/<slug>.md`. |
| Card storage | Mochi only. No local `cards.yaml`. Inventory recorded in `mastery.yaml` log entries. |
| Prereq blocking | Soft. Advisory only. |
| Provenance | LLM-authored content carries `generated_by` + `generated_at`. |
| Skill location | `skills/<name>/SKILL.md`, packaged as the `ai_tutor` plugin. Surfaced as `ai_tutor:<name>` in slash commands. |

## Open questions

- Numeric mastery scoring (currently qualitative).
- Whiteboard / pen input.
- Cross-domain concept reuse (e.g. linearity in calc + LA + ML).
- Source-curation escalation triggers (heuristics for detecting Claude's own quality failure).
- Voice mode (out of scope for v0; CLI Claude has no voice surface).

## Research basis

Decisions surfaced via synthesis of Khanmigo, PS2 Pal RCT, LearnLM, Mollick, Metacademy, MathAcademy, Andy Matuschak, Bret Victor, Ciechanowski, Distill, Bostock. DeepTutor rejected as sloppy; used only for pattern confirmation. Most architecture patterns were independently confirmed across Metacademy + Claude skills + DeepTutor.
