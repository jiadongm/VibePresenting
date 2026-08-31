---
name: research-presentation-prep
description: Use when creating or editing research presentations — reading groups, research seminars and conference talks — especially xaringan/remark decks for mixed disciplinary audiences. Covers session planning, evidence mapping, incremental drafting, claim provenance, slide layout and render-based visual QA.
---

# Research Presentation Prep

## Core Principle

Prepare the session as a guided learning experience, not a compressed recital. For a reading group, the paper is the destination; for a research seminar, the research program is. In both cases, build the conceptual road that lets the audience arrive there.

For interdisciplinary audiences, first identify what they already know and what they likely do not. Spend slide time on the missing conceptual bridge. Put extra technical detail in a handout or speaker notes.

## Recommended Workflow

### For any research talk

1. Read local instructions, the current source and any plan before changing anything.
2. Create a session brief: audience, duration, desired takeaway, central thesis and constraints.
3. Inspect the sources and assign each a role: worked example, conceptual vocabulary, method, broader framing or counterexample.
4. Build a teaching arc and time budget before drafting. Use a handout when it helps with technical detail; it is optional rather than a prerequisite for slides.
5. Draft one section at a time. After each section, invite conceptual and editorial challenge before treating it as accepted.
6. Keep slides sparse; put qualifications, definitions and source nuance in speaker notes or a source map.
7. Before editing, locate the slide by title and nearby content as well as page number. Page numbers change after insertions.
8. Preserve manual edits and provisionally locked slides unless the user explicitly reopens them.
9. Render and visually inspect every material change at the intended presentation size.
10. At the end, reconcile slide count with the time budget and test the opening and closing slides as a pair.

### Source and decision discipline

- Keep a source map for consequential claims, numerical results, paper figures and clinical examples.
- Label each claim as a direct source claim, a paraphrase, or an interpretation made for the talk.
- Recheck the primary source when a factual challenge arises. Plausible wording should not silently become a claim attributed to a paper.
- Maintain a short decision record for terminology, framing, accepted revisions and material deliberately removed from the live deck.
- Treat an audience question about a term, metric or example as evidence that the exposition needs revision.

## Optional Factor IV and causal-inference teaching module

### Avoid Stale Dichotomies

Do not frame the talk as "association bad, causation good." A better framing:

- Association identifies relationships and hypotheses.
- Prediction estimates future or held-out outcomes.
- Causation asks whether changing an exposure would change an outcome.

These goals are complementary. Prediction can use causes, consequences, confounders, and proxies. Causal inference is needed when the user wants intervention-relevant interpretation.

### Introduce Terms When They Become Useful

When causal inference is new to the audience, include a short glossary or introduce terms just before they are needed:

- exposure: variable or feature set whose effect is of interest;
- outcome: response variable;
- confounder: a cause of both exposure and outcome;
- instrument: variable that shifts the exposure but has no direct route to the outcome;
- perturbation: broad source of variation in the exposure, not necessarily CRISPR or a direct biological intervention;
- factor: low-dimensional axis summarizing coordinated variation.

Use the vocabulary consistently after introducing it. Do not introduce several competing effect measures or technical labels unless the talk needs their distinction; explain one measure in plain language before relying on it.

### Use Intuition Before Equations

For instrumental variables, start with the idea:

> Use the part of the exposure that can be explained by an external source of variation.

Then introduce the structural equations. This avoids presenting formulas before the audience knows why the objects exist.

### Be Precise About Perturbations and Instruments

In biology, "perturbation" may mean CRISPR, knockout, diet, antibiotics, treatment, genotype, or environment. In causal-inference teaching, define it broadly:

> A perturbation is a source of variation that shifts the exposure.

Then emphasize:

> Not every perturbation is a valid instrument.

Validity requires relevance, exogeneity, and exclusion.

### Use Concrete DAGs

For IV assumptions, a word-based DAG is often better than abstract `Z`, `X`, `y` alone. For example:

- antibiotics `(Z)` -> microbiome `(X)` -> inflammation `(y)`;
- health risk `(H)` -> microbiome `(X)`;
- health risk `(H)` -> inflammation `(y)`;
- dashed health risk `(H)` -> antibiotics `(Z)` as an exogeneity violation;
- dashed antibiotics `(Z)` -> inflammation `(y)` as an exclusion violation.

This makes clear that:

- hidden confounders point to both exposure and outcome;
- IV exogeneity fails if hidden risk drives instrument assignment;
- exclusion fails if the instrument directly affects the outcome outside the modeled exposure.

### Motivate Factors Biologically, Not Only Computationally

Do not present factors only as a fix for `p >> n`. In omics, factors are also biologically natural because microbes, genes, and metabolites act as systems.

Useful phrasing:

> Factor IV shifts the causal unit from isolated features to perturbation-aligned biological programs.

Clarify that feature-level effects reconstructed as `alpha = V kappa` should not automatically be interpreted as direct causal-parent edges from each feature to the outcome.

### Distinguish Factor-Based and Network-Based Causal Views

Network causal inference tries to resolve directed edges, such as `X1 -> X2 -> y`. This is mechanistically appealing but difficult under hidden confounding, compositionality, feedback, and limited sample size.

Factor-based approaches estimate effects of low-dimensional modules or programs. They trade edge-level mechanistic detail for stability and interpretability at the module level.

The two views are complementary:

> Network causal inference asks which variables cause which others. Factor IV asks whether instrument-induced modules of omics variation affect the outcome.

### Keep Critique Constructive

For a learning-focused reading group, critique should clarify assumptions, not derail the session. Phrase limitations as:

> What biological story would make this assumption plausible? What biological story would break it?

This is especially useful for exclusion restrictions.

## Slide Design Lessons

### Content principles

- One headline claim per slide. Prefer dot points over full sentences.
- Prefer diagrams and simple tables over dense equations.
- Use equations only after the intuition is clear. Add dimensions to matrix equations when introducing factorisation.
- Keep language concrete before technical notation. Introduce every new concept with one concrete example before any notation.
- When using paper figures, preserve aspect ratio and crop/resize for the message of the slide.

### Overflow is the #1 failure mode

Xaringan/remark slides have a fixed viewport height (~742px at 16:9). Content that exceeds it silently overflows — there is no warning at render time.

- **Always render after edits** and visually check the changed slide. Do not trust that content will fit.
- When combining a diagram + content below it, budget vertical space explicitly. Reduce margins, padding and font sizes on the upper element to leave room.
- If a slide overflows, the fix is almost always to cut content or tighten spacing — not to shrink font below readability.

### Font size hierarchy

For information-dense slides, use a consistent hierarchy:

| Element | Size |
|---|---|
| Slide titles | 42–56px |
| Diagram node labels / card headings | 22–30px |
| Supporting detail, bullet text | 20–25px |
| Affiliations, metadata, attributions | 13–16px |

Going below roughly 18px for live body text is usually unreadable from the back of a seminar room. If you need smaller text to fit, the slide has too much content.

### Inline styles vs CSS classes

- Use the deck's existing CSS classes first (`.pull-left`, `.pull-right`, `.callout`, `.programme`, etc.).
- Use inline `style` attributes for genuinely tiny one-off adjustments.
- Create a named, slide-scoped class for a substantial one-off layout; it is safer and easier to inspect than a long inline style.
- Create a reusable component class when a visual pattern recurs. Use modifier classes for compact or slide-specific variants rather than changing a global component rule.

### HTML grid layouts

For collaborator panels, comparison cards and multi-item rows, use CSS grid directly:

```html
<div style="display:grid; grid-template-columns: repeat(N, 1fr); gap:12px; text-align:center;">
  <div>
    <img src="figs/people/Name.jpg"
         style="height:72px; width:72px; object-fit:cover; border-radius:50%;">
    <div style="font-size:14px; margin-top:4px;">Name</div>
    <div style="font-size:11px; color:#5f6b7a;">Affiliation</div>
    <div style="font-size:11px; color:#5f6b7a;">Role</div>
  </div>
  <!-- more items -->
</div>
```

For missing headshots, use a grey circle with initials:

```html
<div style="height:72px; width:72px; border-radius:50%; background:#d0d5dd;
            display:flex; align-items:center; justify-content:center;
            margin:0 auto; font-size:24px; color:#6b7280;">AB</div>
```

### Backbone diagrams as navigation devices

When a talk has a unifying programme diagram (e.g., forward projection → inverse problem), show it at the opening, re-show a compact version at each major section transition, and close with it. This orients the audience without words.

- On first appearance, build in stages if the order matters. On re-appearances, show it static.
- Colour-code method names in the diagram (e.g., blue for Φ-Space, teal for NeighbourNet/DIVAS, gold for future work), then reuse those colours when referencing those methods on later slides. This creates visual coherence without explicit labels.

### Figures and attribution

- Check whether requested assets already exist locally before creating new ones.
- Use local relative paths from the slide source directory.
- Keep figure attributions short and visually quiet — tiny muted text near the figure:

```html
<div style="font-size:8px; color:#5f6b7a; text-align:right;">
Author et al., Journal, Year
</div>
```

### xaringan/remark specifics

- Hide R code chunks with `echo=FALSE`.
- Test math rendering. If MathJax is unreliable, use Unicode/HTML for simple inline equations.
- Avoid Pandoc image attributes (`{width=80%}`) in xaringan — they render literally. Use `<img>` tags with inline `style` instead.
- For `knitr::include_graphics()`, control size via chunk options (`out.width="85%"`).

## Verification

Render after every material edit. This is non-negotiable — xaringan silently overflows.

```sh
Rscript -e 'rmarkdown::render("slides/deck.Rmd", output_format = "xaringan::moon_reader")'
```

After rendering:
- Open the HTML and navigate to the exact changed slide at the intended 16:9 viewport. A screenshot is preferable when available.
- Check title wrapping, overflow, card height, whitespace, source notes, footers and the transition from the preceding slide.
- Check that a CSS change did not disturb unrelated slides.
- If browser/screenshot verification is unavailable, inspect the generated HTML source around the changed slide and record that visual QA remains outstanding.
- Do not assume a slide fits because the content looks short in the `.Rmd` source. HTML elements with padding, margins and grid gaps consume more vertical space than they appear to in source.

## Editing Guardrails

- Do not churn global CSS for one slide.
- Do not replace a figure with a placeholder unless the user asks.
- Do not change slide count, section structure or title/abstract without explicit instruction.
- Do not silently remove citations or attributions.
- Keep unrelated existing content intact.
- Preserve the deck's spelling convention (Australian: -ise, -our), citation style and colour palette.
- For ongoing decks, treat user comments and recent manual edits as primary specifications. Do not rewrite locked sections unless the user explicitly reopens them.

When in doubt, make the slide source simpler rather than adding abstraction.

## Deliverables Pattern

A good project folder may contain:

- `CLAUDE.md` or `AGENTS.md`: project instructions and slide-by-slide plan;
- `PLAN.md`: audience, thesis, structural outline and time budget;
- `SOURCE_MAP.md`: source, location and status of consequential claims;
- `DECISIONS.md`: accepted framing, manual edits and deliberately excluded material;
- `QA.md`: render and visual-inspection record;
- `handout.Rmd`: optional technical teaching handout;
- `slides/deck.Rmd`: slide source;
- `slides/custom.css`: custom xaringan CSS with colour variables;
- `slides/figs/`: figures, diagrams and supporting images;
- `slides/figs/people/`: collaborator headshots for team panels.
