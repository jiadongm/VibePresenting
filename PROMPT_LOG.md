# Prompt and Work Log

This file records the user prompts used while preparing the reading-group handout and slides, together with a brief description of what Codex did in response. It is intended for transparency for reading-group participants.

## Session Log

| # | User prompt | What Codex did |
|---:|---|---|
| 1 | “We will develop slides for a reading group aiming at reading papers on bioinformatic methods related to microbiome research... First read the paper and make a plan for our handout document. Ask me questions if unsure” | Inspected the folder, identified `main.pdf` and `Supp.pdf`, extracted text, read the paper structure, and created `handout_plan.md` with a proposed tutorial handout outline and questions. |
| 2 | “The framework looks good. Note that you can mention mendelian randomisation as a motivating example... [answers to planning questions]” | Updated `handout_plan.md` to include Mendelian randomisation, a 40-minute/25-slide target, tutorial style, light critique, and more emphasis on causal-inference foundations. |
| 3 | “Now start developing the handout” | Drafted the first full handout as `handout.md`, covering association/prediction/causation, structural equations, IV, MR, Factor IV, simulations, applications, critique, and a slide storyboard. |
| 4 | “I have a side question... when we end the current session and restart it on a different machine tomorrow. How does codex pick up memory from this session” | Explained that files in the synced workspace are the reliable memory, while conversation continuity depends on reopening the same thread. Recommended a README/session note. |
| 5 | “actually change handout.md to Rmd file so that we can render maths better” | Renamed/converted `handout.md` to `handout.Rmd`, added YAML, converted core equations to LaTeX display math, and updated `handout_plan.md`. |
| 6 | “I encountered some error when knitting. Try knitting the Rmd file” | Reproduced the error, found that command-line R could not initially find Pandoc, used RStudio’s bundled Pandoc path, then fixed an R Markdown inline-code issue caused by dimensions like `` `r x r` ``. Successfully rendered `handout.html`. |
| 7 | “OK generate a README.md to save this session” | Created `README.md` documenting the project goal, agreed presentation direction, current files, handout status, render note, known knitting fix, and next steps. |
| 8 | “I've read the handout. Overall it's good... Here are some modifications. Think about these modifications critically...” | Reworked `handout.Rmd`: removed body dividers, reframed association/prediction/causation more subtly, added omics stability/transience, added glossary, moved IV before structural notation, combined high-dimensional IV difficulty with Factor IV, moved workflow/simulations to appendices, combined discussion/critique, and replanned slides. Re-rendered handout. |
| 9 | “OK now draft slides using xaringan, put placeholders if you think a figure is needed...” | Created `slides.Rmd` and `slides.css`, drafted a 25-slide xaringan deck from the revised handout, added placeholders for paper figures, and rendered `slides.html`. |
| 10 | “The formulas are not rendered properly in html” | Diagnosed MathJax rendering issues and converted `slides.Rmd` formulas to Unicode/HTML text for robust local display. Re-rendered `slides.html`. |
| 11 | “what if we want still want to render using latex? create slides_latex.Rmd for that” | Created parallel `slides_latex.Rmd` using LaTeX/MathJax notation and rendered `slides_latex.html`. Updated `README.md` to document both slide versions. |
| 12 | “OK I'll manually fine tune the slides. Let's discuss some details here. First what do we mean by perturbations here?...” | Explained “perturbation” as a broad causal-inference source of variation that shifts exposure \(X\), not only CRISPR/experimental perturbation, and clarified that only some perturbations are valid instruments. |
| 13 | “what about exposure” | Defined exposure as the biological variable or feature set whose effect on the outcome is of interest; distinguished exposure \(X\), instrument \(Z\), outcome \(y\), and factor-level exposure. |
| 14 | “A side question: how is the IV approach here different from counterfactuals in causal machine learning...” | Explained that IV and counterfactual causal ML address causal questions with different identification strategies: causal ML often relies on observed adjustment; IV uses exogenous instrument-induced variation in \(X\). |
| 15 | “You said causation is What would happen under intervention? I feel this is not sufficiently intuitive, can you give an example” | Gave a microbiome/inflammation example contrasting association, prediction, and causation; suggested slide wording: “if we could change \(X\), would \(Y\) change as a result?” |
| 16 | “Would you say exposure is better than intervention here? as we introduced exposure ealier” | Agreed that exposure should be introduced first; intervention can be the real or hypothetical action that changes exposure. Suggested table wording centered on exposure \(X\). |
| 17 | “I need some more differentiation between prediction and causation...” | Explained that high-AUC prediction can use causes, consequences, proxies, or confounders; causal models ask whether changing the exposure changes the outcome. Used *R. gnavus* and smoke-alarm examples. |
| 18 | “I wanted to include a subtitle for why microbiome and metabolomics are ticky slide...” | Suggested clearer subtitle options; recommended “Stable genomes, moving targets.” |
| 19 | “would you say the goal of causal inference is to identify causal parents and their effect sizes?...” | Clarified that causal-parent discovery is one causal task, but causal inference more broadly asks what changes under changes to a system. Explained direct effects, total effects, mediation, and the paper’s structured effect-estimation goal. |
| 20 | “actually i think this is a vital problem for understanding here... factors?... network-based approaches...” | Explained that factors are motivated biologically as perturbation-aligned modules/programs, not only computationally. Compared factor-based causal estimation with network-based causal inference. |
| 21 | “OK update our handout with this key discussion point. Motivate factors with this intuition” | Updated `handout.Rmd` to motivate factors as coordinated biological programs/modules, clarified that `alpha = V kappa` is not direct causal-parent discovery, added a factor-vs-network discussion section, and re-rendered handout. |
| 22 | “OK i've done some changes to slides. I'll need your helpf with DAGs... first include H as pointing to both X and Y... for IV intuition page, also include H...” | Replaced simple flow diagrams with inline SVG DAGs in both slide versions: confounding DAG with `H -> X`, `H -> y`, `X -> y`; IV DAG with `Z -> X -> y` plus `H -> X` and `H -> y`. Added CSS and re-rendered. |
| 23 | “can you reduce the legnth of arrows from H to other nodes to save space” | Adjusted SVG geometry to move `H` closer to `X` and `y`, shortened the arrows, reduced figure height, and re-rendered both slide decks. |
| 24 | “there's often not enough space between a table and a callout structure. how do I increase” | Suggested CSS margin rules for `.callout` and `.mini-table + .callout`. |
| 25 | “Ok edit css” | Added callout spacing rules to `slides.css` and re-rendered both slide decks. |
| 26 | “For exogeneiry you used example Was treatment assigned because of risk? can you eloborate on that” | Explained IV exogeneity with an antibiotics example: antibiotics may be non-exogenous if assigned because subjects are already high-risk or inflamed. Suggested clearer slide wording. |
| 27 | “OK let's add a dag using your example to explain the three assumptions, instead of x y z, let's use words in the dag...” | Replaced the IV-assumptions table with a compact word-based antibiotics DAG showing relevance, exogeneity violation, and exclusion violation; used smaller nodes and fonts; re-rendered. |
| 28 | “I like your table earlier. Keep it and delete the callout” | Restored a compact assumptions table below the antibiotics DAG and removed the callout. Adjusted SVG height and re-rendered. |
| 29 | “question: by hidden cause you don't mean confounder right? H should only influence y but not X” | Clarified that in the confounding DAG, \(H\) means an unobserved confounder and points to both \(X\) and \(y\). If \(H\) only points to \(y\), it is an unobserved outcome cause, not a confounder of \(X \rightarrow y\). |
| 30 | “But then if H and Z both point to X then H and Z will be associated?... is health risk causes microbiome?” | Explained collider structure \(H \rightarrow X \leftarrow Z\), why \(H\) and \(Z\) are not necessarily associated, and how exogeneity fails if \(H \rightarrow Z\). Recommended adding `H -> X` and dashed `H -> Z`. |
| 31 | “sounds good” | Updated the IV assumptions DAG to include `health risk -> microbiome`, kept dashed `health risk -> antibiotics` as exogeneity violation, revised table wording, and re-rendered. |
| 32 | “Looking at your example, exclusion seems violated? as antibiotics will tend to influence inflamation direclty” | Agreed and explained that antibiotics are pedagogically useful but biologically suspicious as instruments because direct antibiotic effects on inflammation are plausible. Connected this to multi-omics helping exclusion. |
| 33 | “on page 13 (low-rank first stage), change 'Piece' to 'notation' and remind the reader their dimensions... Question: on page 15, would it be better to say kappa = V^T alpha?” | Updated slides to replace “Piece” with “Notation,” added dimensions for \(U,D,V\), added dimensions for \(F = ZUD\), re-rendered, and explained when \(\kappa = V^\top\alpha\) is valid versus why \(\alpha = V\kappa\) is better for explaining output. |
| 34 | “in your discussion Why multi-omics may help exclusion, do you mean we can find factors of micribiome and metabolome, or joint factors of them? Did the paper actually do that” | Clarified that the HCC analysis uses a joint \(X = [X_{\text{microbiome}}, X_{\text{metabolome}}]\), so Factor IV learns joint microbiome-metabolome factors. |
| 35 | “OK for last page, include 5 headshots in figs/headshots to show the team and their names, scale the photos so that the slide is not too crowded” | Added a compact circular five-person headshot grid to the final slide, styled it in `slides.css`, and re-rendered both decks. |
| 36 | “That's a nice way of showing photos. Under our photos, add violin, violin, viola, cello and piano...” | Added a smaller second row of circular instrument images labeled violin, violin, viola, cello, piano to present the work group as a piano quintet. Re-rendered both decks. |
| 37 | “For infant gut figure, correct my code to include the figure but not the code. resize the figure to fit” | Fixed the R chunk by adding `echo=FALSE`, `out.width="100%"`, a `.case-figure` wrapper, CSS image constraints, and replaced the placeholder in the LaTeX deck. Re-rendered both decks. |
| 38 | “Good. To wrap up, generate a skill.md file documenting high level lessons you learned from this session for preparing presentation for reading groups. What will you put in it” | Created `SKILL.md` with a reusable workflow and lessons for preparing technical reading-group presentations, including handout-first workflow, causal teaching principles, DAGs, factor motivation, critique style, and xaringan lessons. |
| 39 | “OK document all my prompts and briefly record what you did for each prompt, as another md file. This is for transparency, to be distributed to reading group participants.” | Created this `PROMPT_LOG.md` file documenting the user prompts and corresponding actions. |

## Files Created or Substantially Edited

- `handout_plan.md`
- `handout.Rmd`
- `handout.html`
- `handout.pdf`
- `slides.Rmd`
- `slides_latex.Rmd`
- `slides.css`
- `slides.html`
- `slides_latex.html`
- `README.md`
- `SKILL.md`
- `PROMPT_LOG.md`

