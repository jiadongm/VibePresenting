# VibePresenting workflow

Use this workflow for research seminars, journal clubs and conference talks developed with AI agents.

## 1. Start with the session, not the slides

Write a short session brief before drafting:

- audience and their likely starting point;
- duration and discussion time;
- one desired audience takeaway;
- central thesis or recurring question;
- constraints: source papers, format, style and presentation environment.

Use [`PLAN_TEMPLATE.md`](PLAN_TEMPLATE.md) to make these decisions explicit.

## 2. Map the evidence

Read the relevant sources before asserting that a paper makes a particular claim. For each paper, identify its role:

- narrative backbone or worked example;
- conceptual vocabulary or method;
- broader framing, limitation or counterexample.

Record consequential claims in a source map. Mark whether each statement is a direct source claim, a paraphrase, or an interpretation for the talk.

## 3. Agree a teaching arc and time budget

Design the talk around the audience's path, rather than the order of a paper's sections. A useful arc is often:

> problem → concrete example → framing question → design or method → assumptions → limitations → return to the audience's work → closing claim.

Decide what belongs live, in speaker notes, in a handout, or nowhere. A handout is valuable for technical derivations and extensive terminology; it is optional rather than a prerequisite for slides.

## 4. Draft and approve one section at a time

Do not generate an entire deck and treat it as settled. Work in short sections:

1. propose the next section and its job in the narrative;
2. draft the slides sparsely, with notes for spoken nuance;
3. discuss conceptual friction, unfamiliar terminology and examples;
4. revise only the slides under discussion;
5. record accepted framing and lock it provisionally.

Treat the current slide source and the user's manual edits as authoritative. Identify a slide by its title and nearby content as well as page number, because slide numbers change after insertions.

## 5. Make assumptions inspectable

For a causal, statistical or computational method, do not merely list assumptions. Show:

- what question the method is trying to answer;
- the design or comparison that would support it;
- the assumptions that connect the data to that answer;
- one concrete scientific story that could violate each assumption.

Use everyday examples before notation. Introduce technical terms at the moment they become useful; audience questions about a metric or term are evidence that the current exposition needs revision.

## 6. Keep scientific provenance separate from slide prose

Sparse slides often need careful speaker notes. Keep the slide's headline simple, but record the source, qualification and interpretation in the source map or notes. Recheck a source when a factual challenge arises; do not let a plausible paraphrase silently become a claim made by the paper.

## 7. Render, inspect and record QA

Successful rendering is necessary but not sufficient. After every material edit:

1. render the deck with its real command;
2. inspect the exact changed slide at the intended aspect ratio and presentation size;
3. check title wrapping, text overflow, padding, footers, source notes and neighbouring-slide transitions;
4. confirm that global CSS changes did not disturb unrelated slides;
5. record what was checked in [`QA_TEMPLATE.md`](QA_TEMPLATE.md).

At the end, reconcile slide count with the time budget and test the opening and closing slides as a pair.

## 8. Preserve a responsible record

Keep the plan, source map, decisions, rendered output and a concise prompt/action log together. The log should document meaningful corrections and disagreements as well as successful edits. The human agent remains responsible for factual accuracy, audience fit and final scientific judgement.
