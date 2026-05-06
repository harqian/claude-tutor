---
generated_by: claude-opus-4-7
generated_at: 2026-05-03
purpose: visualization playbook, read before generating any viz
---

# Visualization playbook

The bar is Ciechanowski-grade interactives. Bret Victor / Distill / 3blue1brown adjacent. Below is the craft you commit to before writing any viz.

## When you generate a visualization

1. Single self-contained HTML file at `out/artifacts/YYYY-MM-DD_HHMMSS_<topic-slug>.html` (UTC or local, pick one and stay consistent; default local).
2. CDN imports for libraries; all JS inline; no external assets, no build step.
3. Tell Harrison: "look at `localhost:5500/artifacts/<filename>`".
4. **Never overwrite a previous artifact.** Each viz gets a fresh timestamped file. Full history preserved.
5. Optionally append an entry to `out/INDEX.md` with date, topic, one-line description.

## Library defaults

| Use case | Default | Notes |
|---|---|---|
| Math typesetting in prose | KaTeX | faster than MathJax, good enough |
| 2D math: graphs, transforms, vector fields | D3 + SVG | crisp text, easy CSS |
| 2D physics, springs, particles, mouse-drag | p5.js | built-in vectors |
| 3D math, surfaces, manifolds | MathBox | (over Three.js) for math-y 3D |
| Generic 3D, mechanism animation | Three.js | only when MathBox is wrong tool |
| Statistical relationships, parameter studies | Observable Plot or D3 | NOT Plotly (too much chrome) |
| Graphs, trees, data structures | D3 force/tree/hierarchy | Cytoscape.js if >500 nodes |
| Animated math (3blue1brown style) | Manim CLI | render MP4, embed in HTML |
| Diagrams (state machines, flows, ER) | claude-mermaid MCP if installed; else mermaid CDN | |

**Reach order:** SVG/D3 → canvas/p5.js → WebGL/Three.js. Stay at the lowest level that does the job.

## 15 craft principles

1. **The explanation IS the visualization.** Prose introduces a noun; the diagram immediately becomes that noun, grabbable.
2. **Direct manipulation everywhere.** The geometry is the control surface; sliders are secondary, only for parameters with no spatial home.
3. **Climb the ladder of abstraction in both directions.** Pair every aggregate plot with a way to point at one curve and see the underlying instance.
4. **Introduce one variable at a time.** Build N demos, each adding one slider; the final demo is the integration. Never dump all sliders into demo #1.
5. **Concept first, demo second.** Every interactive begins with a one-sentence problem framing immediately above it.
6. **Color is semantic, not decorative.** Pick 4-6 hues, assign each a meaning at the start, never reuse.
7. **Maximize data-ink ratio.** Delete tick marks, gridlines, legends, titles, panel borders. Replace legends with inline labels colored to match the line.
8. **Geometric truth over decorative simplification.** Show what the formula actually does. Do not fudge geometry for prettiness.
9. **Animate to reveal, not to decorate.** Animation for transformations between states; static small multiples for comparisons.
10. **Pause/play and time-scrub controls are mandatory.** Never autoplay loops without controls.
11. **Coordinated multiple views.** A single state object drives N renderers. Dragging in view A updates view B and view C.
12. **Bold inline manipulables, color-matched.** Bold every noun the reader can grab; color the bold to match the on-canvas color.
13. **Treat the reader as an active questioner.** Embed prediction prompts and recall checkpoints in prose (mnemonic medium). Do not only show; ask.
14. **Narrative arc carries technical content.** Open with phenomenon, not definition. Never start with definitions.
15. **Distillation is real work.** Budget multiple drafts; the first explanation is rarely the right one.

## Craft anti-patterns (forbid)

- Plotly defaults (modebar, hover chrome, Plotly branding)
- Tooltip-only labels (use bold inline instead)
- Floating control panels disconnected from geometry
- Categorical rainbow palettes without semantic intent
- Gridlines and chart borders by default
- Autoplay loops without pause
- Definition-first openings
- Multi-feature mega-demo as the only demo
- Generic "Click to interact" labels
- Math as image (use KaTeX)
- Mismatched typography between prose and labels
- Three.js scenes with shadows/fog/orbit-controls cargo-culted from product demos

## Pre-flight checklist (run before writing the file)

- [ ] One-sentence framing above each interactive
- [ ] Direct manipulation chosen over sliders where geometric
- [ ] Color palette assigned semantically up-front
- [ ] No gridlines, no legend, inline labels color-matched
- [ ] Pause/play if any animation
- [ ] At least one prediction prompt embedded in prose
- [ ] Bold inline manipulables
- [ ] KaTeX for math, not images

## Starter HTML template

Use this as the skeleton. Strip what you do not need; add libraries by uncommenting.

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>{{TOPIC}}</title>

  <!-- KaTeX -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.css" />
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.js"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/contrib/auto-render.min.js"
    onload="renderMathInElement(document.body,{delimiters:[{left:'$$',right:'$$',display:true},{left:'$',right:'$',display:false}]})"></script>

  <!-- D3 (default 2D) -->
  <script src="https://cdn.jsdelivr.net/npm/d3@7"></script>

  <!-- Optional: p5 / MathBox / Three.js / Observable Plot -->
  <!-- <script src="https://cdn.jsdelivr.net/npm/p5@1.11.0/lib/p5.min.js"></script> -->
  <!-- <script src="https://cdn.jsdelivr.net/npm/three@0.165.0/build/three.min.js"></script> -->
  <!-- <script src="https://cdn.jsdelivr.net/npm/@observablehq/plot@0.6.16/dist/plot.umd.min.js"></script> -->

  <style>
    :root {
      --c-bg: #fafaf7;
      --c-fg: #1a1a1a;
      --c-muted: #777;
      --c-accent-1: #2b6cb0;
      --c-accent-2: #c05621;
      --c-accent-3: #2f855a;
    }
    html, body { background: var(--c-bg); color: var(--c-fg); margin: 0; }
    body { font: 16px/1.55 ui-serif, Georgia, "Iowan Old Style", serif; max-width: 720px; margin: 0 auto; padding: 32px 20px 80px; }
    h1, h2 { font-family: ui-sans-serif, system-ui, -apple-system, "Helvetica Neue", sans-serif; }
    h1 { font-size: 1.6rem; margin: 0 0 0.4rem; }
    h2 { font-size: 1.15rem; margin: 2rem 0 0.6rem; }
    .figure { margin: 1.2rem 0; }
    .grab { color: var(--c-accent-1); font-weight: 600; cursor: ew-resize; }
    .a1 { color: var(--c-accent-1); }
    .a2 { color: var(--c-accent-2); }
    .a3 { color: var(--c-accent-3); }
    svg { display: block; }
    .controls { display: flex; gap: 12px; align-items: center; margin: 8px 0; font-size: 14px; color: var(--c-muted); }
    button { font: inherit; background: transparent; border: 1px solid #ccc; padding: 4px 10px; border-radius: 4px; cursor: pointer; }
    button:hover { background: #eee; }
  </style>
</head>
<body>

<h1>{{TOPIC}}</h1>

<p>One-sentence framing of the phenomenon. No definition yet.</p>

<div class="figure" id="fig-1"></div>

<p>Body prose. Bold the <span class="grab a1">grabbable noun</span> and color-match to the on-canvas element.</p>

<p><em>Predict:</em> what happens if you drag the point past <span class="a1">x = 1</span>?</p>

<script>
  // single state object drives all views
  const state = { x: 0.5 };

  const fig = d3.select("#fig-1");
  const w = 640, h = 280;
  const svg = fig.append("svg").attr("viewBox", `0 0 ${w} ${h}`).attr("width", "100%");

  // ... draw + bind direct-manipulation handlers here ...
</script>

</body>
</html>
```

## Notes on the template

- Serif body, sans headings: matches Distill / Ciechanowski tradition. Readable without looking like a marketing landing page.
- `viewBox` with `width: 100%` instead of fixed pixel sizes: the artifact survives mobile viewport better and lives well in `live-server`.
- `--c-accent-*` CSS variables are the **semantic palette**: assign meaning before writing JS, then reuse via `var(--c-accent-N)`.
- The `state` object pattern (principle 11) is the default. Multiple views read from it; handlers mutate it; a redraw function dispatches.
