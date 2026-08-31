# Vibe presenting: prepare presentations by working with AI agents

This repository has two purposes:

1. it contains materials for a one-hour reading-group session on

> *Moving from Association to Causation: Instrumental factor models for causal inference in high-dimensional multi-omics data*

2. it documents a reusable, human-directed workflow for developing research presentations with AI agents.

Paper DOI for this first case study: <https://doi.org/10.64898/2025.12.26.696613>

The source paper PDFs are **not** redistributed in this repository. Please access the paper through the DOI above.

## What is vibe presenting

I am publishing this repository not only to share the final reading-group materials, but also as a small case study in how I do vibe presenting, i.e. using AI coding/writing agents to prepare technical presentations.

The choice of 'vibe presenting' is deliberate. Vibe coding was coined by Andrej Karpathy to describe how he uses AI agents to do weekends *side projects*. This is how I look at vibe coding:

> **When I work with AI agents, I see them as collaborators, and myself as the human agent. I do my fair share of the work and take final responsibility.**

Vibe coding doesn't (or shouldn't) mean we delegate everything to AI for every task,
while the human user just sips their coffee while clicking 'Yes',
when prompted by agents. Vibe coding, like every responsible use of AI, should involve
a lot of back and forth, and careful review of results by human agents.

The goal is not to ask an agent to “make slides” in one shot. The useful unit of work is a decision loop:

1. Define the audience, duration, desired takeaway and central thesis.
2. Read the sources and map what each contributes to the story.
3. Agree on a narrative and time budget before drafting.
4. Draft one section at a time; the human agent interrogates concepts, wording and examples.
5. Record accepted terminology, source-backed claims and deliberately excluded material.
6. Render and inspect the actual changed slides at presentation size.
7. Revise, then move to the next section.

This can include a handout, but it need not. The Factor IV case used a tutorial handout before slides; a multi-paper journal club may instead begin with a concise plan and develop the deck section by section. In both cases, the human work is in setting the goal, judging the audience, steering the narrative, correcting conceptual emphasis and making fine-grained presentation decisions.

[`WORKFLOW.md`](WORKFLOW.md) gives the reusable workflow. The templates in this repository make its evolving state visible.

## Transparency

For transparency, I have included a prompt/action log:

- [`PROMPT_LOG.md`](PROMPT_LOG.md)

This file records the prompts used during preparation and briefly summarizes what the agent did in response. It is not a perfect transcript. It should be read alongside a plan, decision log, source map and QA record: the transcript alone cannot show every human correction, source check or visual judgement behind a final deck.

## Main Materials

- [`slides.html`](slides.html): main xaringan slide deck, designed to render robustly in browsers.
- [`slides_latex.html`](slides_latex.html): alternative slide deck using MathJax/LaTeX notation.
- [`handout.html`](handout.html): tutorial-style handout.
- [`handout.pdf`](handout.pdf): PDF version of the handout.
- [`index.html`](index.html): GitHub Pages landing page.

If GitHub Pages is enabled for this repository, the landing page should be available at:

``` text
https://jiadongm.github.io/VibePresenting/
```

and the main slide deck at:

``` text
https://jiadongm.github.io/VibePresenting/slides.html
```

## What the Reading Group Covers

The reading group is framed as an introduction to causal inference through the Factor IV paper. The materials cover:

- association, prediction, and causation as distinct but complementary goals;
- why microbiome and metabolomics data are transient and difficult for causal interpretation;
- a glossary of causal-inference terms;
- instrumental-variable intuition before structural notation;
- Mendelian randomisation as a motivating biomedical IV example;
- Factor IV as a way to estimate effects of perturbation-aligned omics programs;
- why factors are biologically motivated, not just computational dimension reduction;
- case studies from DIABIMMUNE and a mouse HCC multi-omics model;
- light critique around IV assumptions, especially exogeneity and exclusion.

## Repository Structure

- `handout.Rmd`: source for the handout.
- `handout.html`, `handout.pdf`: rendered handout.
- `slides.Rmd`: source for the main xaringan deck.
- `slides_latex.Rmd`: source for the MathJax/LaTeX version.
- `slides.css`: custom slide styling.
- `slides.html`, `slides_latex.html`: rendered slide decks.
- `figs/`: figures and supporting images used in the slides.
- `PROMPT_LOG.md`: prompt/action log for transparency.
- `WORKFLOW.md`: reusable human-agent presentation workflow.
- `SKILL.md`: reusable guidance for research-presentation development.
- `PLAN_TEMPLATE.md`: audience, thesis, timing and narrative template.
- `SOURCE_MAP_TEMPLATE.md`: claim-level provenance template.
- `DECISIONS_TEMPLATE.md`: record of accepted framing and locked material.
- `QA_TEMPLATE.md`: render and visual-inspection checklist.

## Rendering Notes

The slides and handout are written in R Markdown/xaringan. On my machine, command-line rendering used RStudio's bundled Pandoc:

``` sh
RSTUDIO_PANDOC=/Applications/RStudio.app/Contents/Resources/app/quarto/bin/tools/aarch64 \
Rscript -e "rmarkdown::render('slides.Rmd'); rmarkdown::render('handout.Rmd')"
```

Inside RStudio, knitting should usually find Pandoc automatically.

There are two slide versions because MathJax can be fragile in local HTML slides:

- `slides.Rmd` uses robust HTML/Unicode notation for formulas.
- `slides_latex.Rmd` uses LaTeX/MathJax for nicer mathematical rendering.

## A Note on Agent Use

This repository should not be read as “AI-generated slides.” It is better described as:

> human-directed, agent-assisted presentation development.

The agent accelerated reading, drafting, refactoring, rendering, and implementation. The final shape reflects repeated human steering: what to emphasize, what to remove, what was conceptually misleading, where the audience needed more intuition, and how the slides should feel in a live reading group.

The record should include corrections and disagreements, not only successful generations. A challenge to a metric, an assumption, a clinical example or a slide's visual density is part of the scientific and editorial work—not a failure of the workflow.
