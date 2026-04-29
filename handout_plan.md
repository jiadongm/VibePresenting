# Handout Plan: Factor IV for Causal Inference in High-Dimensional Multi-Omics

Paper: *Moving from Association to Causation: Instrumental factor models for causal inference in high-dimensional multi-omics data*

Audience: bioinformaticians and statisticians, most with limited causal-inference background.

Note: this file is the original planning scaffold. The current implemented structure and revised slide plan are in `handout.Rmd`, especially Appendix D.

## Proposed Handout Goals

1. Give readers enough causal-inference vocabulary to understand what the paper is trying to estimate.
2. Explain why association-based microbiome/multi-omics analyses can fail as causal evidence.
3. Build from classical instrumental variables to the paper's Factor IV model, using Mendelian randomisation as a familiar biomedical motivating example.
4. Clarify the key assumptions: relevance, exclusion, independence/exogeneity, and rank/identifiability.
5. Explain the algorithm in statistical terms without hiding the biological interpretation.
6. Separate what the method can identify from what still depends on scientific judgment, with a light critique near the end.

## Suggested Length and Style

- Target length: about 8-12 pages as a technical reading-group handout, drafted as `handout.Rmd` so equations render cleanly.
- Teaching-material style: tutorial-like, explanatory, and paced for readers who know biology/statistics but not causal inference.
- Include equations, but keep each equation tied to a plain-language interpretation.
- Use boxed summaries for causal assumptions and "what can go wrong" points.
- Use simple DAGs and matrix-shape tables, since the notation is dense.

## Presentation Translation

- Session length: 1 hour.
- Presentation target: about 40 minutes, leaving about 20 minutes for discussion.
- Slide target: about 25 slides including title.
- Slide emphasis: mostly educational causal-inference foundations, then use Factor IV and the two applications as the worked example.
- Suggested allocation:
  - 3-4 slides: motivation and association vs causation in microbiome research;
  - 7-8 slides: causal inference and IV fundamentals;
  - 2-3 slides: Mendelian randomisation as a biomedical IV example;
  - 5-6 slides: Factor IV model and estimation workflow;
  - 4-5 slides: simulations and biological applications;
  - 1-2 slides: light critique, takeaways, discussion prompts.

## Detailed Outline

### 1. Motivation: Why Causality Is Hard in Microbiome/Multi-Omics

Purpose: orient the audience before introducing IVs.

Content:

- Typical microbiome question: does a taxon/metabolite/pathway causally affect a host phenotype?
- Why ordinary regression, differential abundance, and correlation networks are not causal by default.
- Main failure modes:
  - unobserved confounding, e.g. diet, inflammation, batch, environment;
  - reverse causation, e.g. disease state changing the microbiome;
  - compositional effects and correlated features;
  - high dimensionality, with `p >> n`;
  - multi-layer mediation across microbes, metabolites, immune phenotypes.
- Connect to the paper's examples:
  - infant microbiome and autoimmune outcomes;
  - mouse HCC model with diet, antibiotics, immune knockouts, microbiome, metabolomics.

Figures/tables:

- Simple DAG: `H -> X`, `H -> Y`, `X -> Y`.
- Table contrasting "association question" vs "causal question".

### 2. Causal Estimands and Structural Equations

Purpose: define what the paper wants to estimate.

Content:

- Introduce variables:
  - `Z`: instruments/perturbations;
  - `X`: high-dimensional endogenous omics features;
  - `Y` or `y`: outcome;
  - `Q`: observed covariates/confounders;
  - `H`: unobserved confounders.
- Structural outcome model:

```text
y = X alpha + Q phi + epsilon
```

- Interpret `alpha_j`: causal effect of feature `j`, conditional on the model.
- First-stage model:

```text
X = Z beta + Q psi + E
```

- Explain endogeneity:

```text
Cov(X, epsilon) != 0
```

- Translate into biology: hidden biological/technical processes create dependence between omics features and outcome noise.

### 3. Classical Instrumental Variables

Purpose: teach IV logic before Factor IV.

Content:

- One-exposure, one-instrument example.
- IV intuition: use only the part of `X` shifted by an external perturbation `Z`.
- Mendelian randomisation as a motivating biomedical example:
  - genetic variant as `Z`;
  - modifiable exposure as `X`;
  - disease/phenotype as `Y`;
  - core appeal: genotype is fixed before disease and often less confounded than measured exposures;
  - core warning: pleiotropy is an exclusion-restriction problem.
- Two-stage least squares:
  1. regress `X` on `Z` and `Q`;
  2. regress `y` on predicted/instrumented `X` and `Q`.
- Assumptions:
  - relevance: `Z` changes `X`;
  - exclusion: `Z` affects `y` only through `X`;
  - independence/exogeneity: `Z` is unrelated to unobserved causes of `y`;
  - rank/identifiability: enough independent instrument variation.
- Weak instruments and invalid instruments.
- Why classical IV breaks for multi-omics:
  - too many exposures;
  - too many instruments;
  - sparse/count/compositional data;
  - correlated biological programs rather than isolated features.

Figures/tables:

- IV DAG from the paper, redrawing in handout style.
- Assumption table with biological examples and potential violations.

### 4. From IV to Factor IV

Purpose: explain the paper's main methodological move.

Content:

- The problem: direct estimation of `beta` is unstable when `X` and/or `Z` are high-dimensional.
- Factor IV assumption:

```text
beta = U D V^T,    rank(beta) = r << min(d, p)
```

- Interpretation:
  - `U`: which instruments define each latent perturbation axis;
  - `D`: strength of each axis;
  - `V`: which omics features load on each axis;
  - `F = Z U D`: instrumental factors, i.e. low-dimensional instrument-induced biological variation.
- First-stage representation:

```text
X = F V^T + Q psi + E
```

- Second-stage representation:

```text
y = F kappa + Q phi + error
```

- Map factor-level effects back to feature-level effects:

```text
alpha = V kappa
```

- Key conceptual message: Factor IV estimates causal effects of structured, perturbation-aligned biological programs, then projects those effects onto individual features.

Figures/tables:

- Matrix-shape table for `Z`, `X`, `Q`, `U`, `D`, `V`, `F`, `kappa`, `alpha`.
- Diagram of `Z -> F -> X -> y` with `V` connecting factors to features.

### 5. Identification and Assumptions in Factor IV

Purpose: explain what makes the factors valid instruments, and where skepticism belongs.

Content:

- Since `F = Z U D`, if `Z` is exogenous, then `F` inherits exogeneity under fixed first-stage parameters:

```text
Cov(F, epsilon | Q) = 0
```

- Relevance becomes `rank(Z beta) = r > 0`.
- Low-rank structure is not just computational: it is part of identifiability in high dimensions.
- Per-feature effects require the structural restriction `alpha = V kappa`.
- Explain the supplement's point that latent confounding and correlated first-/second-stage errors are two observed-data representations of endogeneity.
- Discuss multi-omics integration:
  - adding omics layers may make exclusion more plausible if omitted molecular mediators are now measured;
  - but it does not prove exclusion.

Critical notes:

- Exclusion and independence remain scientific assumptions, not empirically testable facts.
- If instruments directly affect outcomes through unmeasured non-omics paths, estimates may be biased.
- Feature-level `alpha` is structured through `V`; it is not an unrestricted per-feature causal coefficient vector.
- Factor interpretability depends on sparsity, rank choice, preprocessing, and stability.

### 6. Estimation Workflow

Purpose: give a step-by-step algorithm readers can map to code.

Content:

1. Preprocess omics data.
2. Define instruments `Z`, observed covariates `Q`, endogenous features `X`, and outcome `y`.
3. Fit sparse low-rank first-stage model:
   - GOFAR for Gaussian/approximately Gaussian features;
   - NBFAR for negative-binomial count features.
4. Construct `F_hat = Z U_hat D_hat`.
5. Fit second-stage regression of `y` on `F_hat` and `Q`.
6. Estimate feature-level effects `alpha_hat = V_hat kappa_hat`.
7. Interpret factor loadings, instrument contributions, and feature-level effects.

Include:

- Pseudocode block.
- Notes on GLM first-stage models for count data:

```text
X_ij | Z_i, Q_i ~ NB(mu_ij, phi_j)
log(mu_ij) = Z_i^T beta_j + Q_i^T psi_j
```

- Brief mention of logistic/GLM second-stage models for binary/count outcomes.

### 7. Simulation Study

Purpose: explain what the simulations validate and what they do not.

Content:

- Simulation design:
  - `n = 200`, `d = 20`, `p = 100`, `t = 3`, `k = 3`;
  - sparse low-rank `beta = U D V^T`;
  - Gaussian and negative-binomial `X`;
  - endogeneity from either unobserved confounding or correlated errors;
  - signal-to-noise ratios controlled.
- Benchmarks:
  - naive feature-wise OLS for continuous features;
  - DESeq2-style association for count features.
- Results:
  - Factor IV recovers `y`, `beta`, projected causal signal, and `alpha` better than association baselines.
- Interpretation:
  - strong evidence the implementation works when the data-generating process matches the model;
  - simulations do not validate IV assumptions in real applications.

### 8. Application 1: DIABIMMUNE Infant Microbiome

Purpose: show how Factor IV is used in a longitudinal human microbiome setting.

Content:

- Scientific setting: early-life microbiome, HLA risk, islet autoantibodies/T1D.
- Data strategy:
  - age-stratified windows: 0-12, 12-24, >24 months;
  - one sample per participant per window;
  - microbiome features as `X`;
  - early-life variables as `Z`;
  - demographic/technical variables as `Q`.
- Instruments:
  - HLA risk, delivery mode, feeding mode, environmental exposures.
- Main findings:
  - HLA dominates early microbiome factors;
  - environmental factors matter more later;
  - Ruminococcus gnavus and Streptococcus appear risk-associated;
  - Alistipes shahii and Akkermansia muciniphila appear protective.
- Critical discussion:
  - Are HLA and early-life variables valid instruments?
  - Could they affect immune outcomes through non-microbial pathways?
  - Does age stratification adequately handle longitudinal dependence?

### 9. Application 2: Mouse HCC Multi-Omics

Purpose: show Factor IV in a perturbation-rich experimental multi-omics design.

Content:

- Scientific setting: murine hepatocellular carcinoma with microbiome, metabolomics, immune perturbations, diet, antibiotics.
- Instruments:
  - high-fat diet, antibiotics, immune-related knockouts.
- Observed covariates:
  - age, strain, body weight.
- Endogenous features:
  - microbiome and metabolomics concatenated into `X`.
- Method:
  - CLR/log-normalized features;
  - GOFAR first stage;
  - rank fixed at `r = 2`;
  - second-stage models for obesity, NASH, HCC, immune phenotypes.
- Main findings:
  - dominant V1 axis driven by diet/antibiotics;
  - V1 associated with obesity, NASH, HCC;
  - signals point toward Ruminococcus gnavus, Clostridia, bile acids/lipid metabolism;
  - DESeq2 highlights different taxa, illustrating association vs instrument-aligned causal signal.
- Critical discussion:
  - Diet and antibiotics plausibly have direct host effects, so exclusion may be delicate.
  - Multi-omics integration may reduce omitted-mediator bias but cannot remove all direct paths.

### 10. How to Read the Paper's Claims

Purpose: help the reading group assess the contribution fairly.

Content:

- What is statistically novel/useful:
  - IV adapted to high-dimensional multi-omics;
  - supervised sparse low-rank first stage;
  - interpretable factor-level and feature-level effects;
  - GLM/NB extension for count-like data.
- What is biological/statistical caution:
  - causal interpretation depends on instrument validity;
  - rank and sparsity choices influence factors;
  - feature-level effects are reconstructed, not directly estimated feature by feature;
  - uncertainty propagation from first stage to second stage needs careful handling;
  - compositional data issues are handled through preprocessing/working models, not fully solved.
- Tone for the session:
  - keep critique light and educational;
  - frame limitations as "what assumptions would we need to believe?" rather than as a takedown;
  - reserve deeper skepticism for discussion after the main tutorial arc.

### 11. Discussion Questions for the Reading Group

Possible questions:

1. In each application, which IV assumption is most questionable?
2. Are the learned factors better interpreted as causes, mediators, or instrument-induced summaries?
3. When is `alpha = V kappa` biologically plausible?
4. Does adding more omics layers strengthen exclusion, or can it introduce new complications?
5. What negative controls or sensitivity analyses would increase confidence?
6. How would this compare to Mendelian randomization, mediation analysis, or longitudinal causal models?

## Proposed Handout Artifacts

- Box 1: Causal inference vocabulary.
- Box 2: IV assumptions with microbiome examples.
- Box 3: Factor IV algorithm.
- Table 1: Variables and matrix dimensions.
- Table 2: Classical IV vs Factor IV.
- Figure 1: Confounding DAG and IV DAG.
- Figure 2: Factor IV matrix decomposition.
- Figure 3: Application summary table.
- Final section: "What I would ask the authors."

## Confirmed Direction

- The handout should be tutorial-like teaching material.
- The slides should be educational, with more time on causal-inference foundations than biological background.
- The critique should be present but light, placed toward the end.
- The biological context can be concise because the audience will already be comfortable with microbiome and bioinformatics concepts.
