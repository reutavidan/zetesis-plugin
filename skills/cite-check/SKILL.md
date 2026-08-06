---
description: Check whether each citation in a text actually supports the sentence it is attached to, and flag identifiers that do not resolve.
disable-model-invocation: true
argument-hint: "[file or passage]"
---

# Check citations against what they are cited for

Target: $ARGUMENTS (a file, a passage, or the document in context).

A citation fails in three distinct ways, and they need different handling:

- **It does not exist.** The identifier resolves to nothing, or to a different paper
  than the one named. This is the failure mode that matters most when text was drafted
  with a language model, since plausible-looking identifiers are easy to generate.
- **It exists but does not say that.** Real paper, wrong support. Often it is adjacent
  work, a review citing the actual source, or the right group's different study.
- **It exists and is about the right thing, but does not carry the weight.** A
  mechanism paper cited for a clinical outcome, a preprint cited as settled, an animal
  result cited for a human claim, a single arm cited as a comparison.

## What to do

1. Extract every citation and the specific sentence each one is attached to.
2. For each, run `zetesis_evidence` with `as_of` set to the cited year and three to six
   short queries, in this order. Titles, author lists, and study-design phrases retrieve
   commentary instead of the paper, so never pass them.
   - The identifier alone, bare: `10.1056/NEJMoa2034577`, or `33301246`. No `PMID:` or
     `doi:` prefix. This is the highest-yield query and usually resolves on its own.
   - The entity alone: the drug, compound code, gene, or model. One or two words.
   - That entity plus at most two words naming the outcome.
   - Optionally the disease or endpoint alone.
3. Look for the paper in the returned bundle by hard id, then compare what the retrieved
   record says against what the sentence claims from it.
4. Classify: **resolves and supports**, **resolves but does not support the sentence**,
   **resolves but is too weak for the claim**, or **could not be retrieved**.

## Reporting

One line per citation, grouped by classification, worst first. For anything that is not
a clean pass, quote the sentence and say in one line what the source actually supports.

Be careful with "could not be retrieved". Use it only after both the bare-identifier
query and the entity-anchored queries have missed. It means the search did not surface
it, which is not the same as the paper not existing: coverage is not complete, older and
non-indexed work is patchy, and a book or a thesis will usually not appear. Say "not
retrieved" and let the author confirm, rather than declaring a citation fabricated. If
the bundle reports that retrieval was degraded for a source, withhold the classification
for that citation instead of recording a failure. Be direct about the ones you did
confirm are wrong.
