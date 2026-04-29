---
name: reading-group-presentation-prep
description: Use when preparing technical reading-group presentations from papers, especially for mixed bioinformatics/statistics audiences where the presenter needs a teaching handout, xaringan slides, careful conceptual scaffolding, and nuanced discussion of assumptions rather than a simple paper summary.
---

# Reading Group Presentation Prep

## Core Principle

Prepare the session as a guided learning experience, not a compressed paper recital. The paper is the destination; the talk should build the conceptual road that lets the audience arrive there.

For interdisciplinary reading groups, first identify what the audience already knows and what they likely do not. Spend slide time on the missing conceptual bridge. Put extra technical detail in a handout.

## Recommended Workflow

1. Inspect the folder and identify the main paper, supplement, existing figures, and any prior notes.
2. Extract or skim the paper structure: abstract, introduction, methods, results, supplement sections, figures.
3. Draft a handout before slides.
   - Use the handout for conceptual scaffolding, equations, assumptions, terminology, and caveats.
   - Keep it tutorial-like when the audience is new to the method.
4. Reorganize the paper into a teaching arc.
   - Motivation and vocabulary first.
   - Intuition before notation.
   - Notation before algorithm.
   - Applications after the audience knows what to look for.
   - Critique near the end unless the user asks for a review-style talk.
5. Draft slides from the handout, not directly from the paper.
   - Slides should be sparse and spoken-through.
   - Use placeholders for paper figures if extraction is not yet done.
   - Keep appendix material in the handout unless it is needed live.
6. Render early and often.
   - For R Markdown/xaringan, check that code is hidden, math renders, figures fit, and CSS spacing works.
   - Preserve the user's manual edits by inspecting current files before patching.

## Teaching Lessons

### Avoid Stale Dichotomies

Do not frame the talk as "association bad, causation good." A better framing:

- Association identifies relationships and hypotheses.
- Prediction estimates future or held-out outcomes.
- Causation asks whether changing an exposure would change an outcome.

These goals are complementary. Prediction can use causes, consequences, confounders, and proxies. Causal inference is needed when the user wants intervention-relevant interpretation.

### Define Terms Early

When causal inference is new to the audience, include a short glossary before heavy notation:

- exposure: variable or feature set whose effect is of interest;
- outcome: response variable;
- confounder: a cause of both exposure and outcome;
- instrument: variable that shifts the exposure but has no direct route to the outcome;
- perturbation: broad source of variation in the exposure, not necessarily CRISPR or a direct biological intervention;
- factor: low-dimensional axis summarizing coordinated variation.

Use the vocabulary consistently after introducing it.

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

- Prefer diagrams and simple tables over dense equations.
- Use equations only after the intuition is clear.
- Add dimensions to matrix equations when introducing factorization.
- For xaringan, test math rendering. If MathJax is unreliable, use Unicode/HTML for simple equations or create a parallel LaTeX version.
- Hide R code chunks with `echo=FALSE`.
- Wrap large figures in CSS classes and constrain `max-height` to avoid overflowing slides.
- Add reusable CSS for callouts, figure placeholders, DAGs, team grids, and spacing.
- When using paper figures, preserve aspect ratio and crop/resize for the message of the slide.

## Deliverables Pattern

A good project folder may contain:

- `README.md`: project state and next steps;
- `handout.Rmd`: technical teaching handout;
- `handout.html` / `handout.pdf`: rendered handout;
- `slides.Rmd`: robust slide deck;
- `slides_latex.Rmd`: optional MathJax/LaTeX slide deck;
- `slides.css`: custom xaringan CSS;
- `figs/`: extracted paper figures, diagrams, and supporting images.

