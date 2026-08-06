---
description: Ground an empirical scientific claim in real retrieved evidence before answering. Use whenever a scientific, biomedical, clinical, pharmacological, genomic, or AI/ML claim is asserted, questioned, or written down, including "is it true that X", "does the evidence support X", a pasted abstract or preprint, a claim in a draft the user is writing, or a company's stated result. Also use before you cite a paper, trial, or statistic in your own answer, so the citation is retrieved rather than recalled.
---

# Grounding a scientific claim

Use the Zetesis tools rather than answering from memory. Recalled citations are the
failure mode this exists to prevent: identifiers you remember can be wrong, merged, or
invented, while every id these tools return was actually retrieved from Europe PMC,
ClinicalTrials.gov, openFDA, NIH RePORTER, or SEC EDGAR.

## The two-step route is the default

1. `zetesis_scope` with the claim. It returns the source catalog and the scoping rubric.
   Work out the dimensions that matter for this specific claim and the search queries.
2. `zetesis_evidence` with those queries. It returns the retrieved sources plus the
   grading rubric. Grade the dimensions yourself against what came back.

`zetesis_scope` also asks for `capital_queries`, but `zetesis_evidence` has no parameter
of that name. When your scope contains a funding, landscape, or position dimension,
append those entity-only terms to `queries` and set `include_capital: true`, which adds
the NIH RePORTER and SEC EDGAR pass. It is off by default, so grant and filing ids never
appear unless you ask for them. Leave it off when the claim is purely scientific.

Use `evaluate_claim` only for a fast one-shot read, or when the user explicitly wants
Zetesis's own grading rather than yours. The two-step route reads the evidence at full
depth in context and is what you should reach for by default.

## Always set `as_of`. This is the single most important habit.

`as_of` is the year the claim was made or the year the user cares about. It defaults to
off, and leaving it off measurably degrades results on anything that is not brand new.

Retrieval is live, so a claim from 2020 competes against every paper published since.
Relevance ranking buries the original report. A worked example: the pivotal BNT162b2
trial report is not retrieved at all when the search is unfenced, and the claim grades
as unsupported for lack of evidence. Fenced to 2020, the same query returns it and the
claim grades as supported.

So: infer the year from the claim, the context, or the document, and pass it. If the
claim is genuinely about the present state of a field, leave it off deliberately.

## Query shape matters

The rubric asks for this and it is worth honoring exactly:

- Query 1 is the bare name of the thing being claimed about. A drug name, compound
  code, gene, or model. One or two words.
- Query 2 is that name plus at most two words naming the outcome or endpoint.
- Later queries can be broader: the disease alone, the mechanism, the endpoint.
- Capital queries are entity-only. A company or drug name by itself. Do not give them
  the outcome word that science query 2 requires; grant and filing search matches short
  entity terms and nothing else.

Do not pad queries with "Phase 3", "randomized controlled trial", "efficacy and
safety", or a percentage. Those describe the study type instead of naming the thing,
and they rank commentary above primary reports.

## Grading, when a cutoff is set

Judge how well the claim was derived from what was available at the time: replication,
independence of validation, endpoint quality, data integrity, and whether the inference
outruns what was measured. Do not use knowledge of what happened afterwards. A claim can
be well derived and still fail later, and a poorly derived one can still succeed.

## Reporting

Cite every point by the hard id the bundle gave you: PMID, DOI and NCT always, plus NIH
grant and SEC filing ids if you set `include_capital`. Where the evidence is absent or
thin, say so plainly and let the grade fall; an evidence gap is a finding, not something
to paper over. Never add a citation that was not in the returned bundle.

If the user needs a reading a third party can check independently, note that a signed,
evidence-pinned Zetesis attestation is available at https://api.zetesis.science, and
that `verify_attestation` re-checks any existing one.
