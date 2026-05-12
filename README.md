# Vibe presenting: prepare presentations by working with AI agents

This repository contains materials for a one-hour reading-group session on:

> *Moving from Association to Causation: Instrumental factor models for causal inference in high-dimensional multi-omics data*

Paper DOI: <https://doi.org/10.64898/2025.12.26.696613>

The source paper PDFs are **not** redistributed in this repository. Please access the paper through the DOI above.

## What is vibe presenting

I am publishing this repository not only to share the final reading-group materials, but also as a small case study in how I do vibe presenting, i.e. using AI coding/writing agents to prepare technical presentations.

The choice of 'vibe presenting' is deliberate. Vibe coding was coined by Andrej Karpathy to describe how he uses AI agents to do weekends *side projects*. This is how I look at vibe coding:

> **When I work with AI agents, I see them as collaborators, and myself as the human agent. I do my fair share of the work and take final responsibility.**

Vibe coding doesn't (or shouldn't) mean we delegate everything to AI for every task, 
while the human user just sips their coffee while clicking 'Yes', 
when prompted by agents. Vibe coding, like every responsible use of AI, should involves 
a lot of back and forth, and careful review of results by human agents.

When it comes to vibe presenting, the goal was not to ask an agent to “make slides” in one shot. Instead, the process was deliberately iterative:

1.  I chose the paper and set the audience: bioinformaticians and statisticians, most of whom would not be familiar with causal inference.
2.  The agent read the paper and supplement, then proposed a handout plan.
3.  I steered the framing: more educational, less paper-summary-like, with more causal-inference basics.
4.  The agent drafted a tutorial-style handout.
5.  I read it and pushed back on wording, structure, missing concepts, and over-simple framing.
6.  We iteratively refined the conceptual arc, especially around association vs prediction vs causation, instrumental variables, perturbations, exposure, confounding, and factors.
7.  The agent generated xaringan slides from the handout.
8.  I manually fine-tuned the slides and asked for targeted changes: DAGs, CSS spacing, figure sizing, final team slide, and assumptions diagrams.
9.  The agent rendered, debugged, and updated files as the presentation evolved.

In other words, these materials are the result of back-and-forth collaboration. The agent helped with reading, drafting, restructuring, rendering, and implementation details. The human work was in setting the goal, judging the audience, steering the narrative, correcting conceptual emphasis, and making many fine-grained presentation decisions.

## Transparency

For transparency, I have included a prompt/action log:

- [`PROMPT_LOG.md`](PROMPT_LOG.md)

This file records the prompts used during preparation and briefly summarizes what the agent did in response. It is not a perfect transcript, but it gives readers a clear view of the human-agent workflow behind the final artifacts.

## Main Materials

- [`slides.html`](slides.html): main xaringan slide deck, designed to render robustly in browsers.
- [`slides_latex.html`](slides_latex.html): alternative slide deck using MathJax/LaTeX notation.
- [`handout.html`](handout.html): tutorial-style handout.
- [`handout.pdf`](handout.pdf): PDF version of the handout.
- [`index.html`](index.html): GitHub Pages landing page.

If GitHub Pages is enabled for this repository, the landing page should be available at:

``` text
https://<username>.github.io/<repo>/
```

and the main slide deck at:

``` text
https://<username>.github.io/<repo>/slides.html
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
- `SKILL.md`: reusable lessons learned for preparing future reading-group presentations with agents.

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
