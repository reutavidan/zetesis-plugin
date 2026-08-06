---
description: Check the empirical claims in a document against retrieved literature and report which hold, which are thin, and which have no support.
disable-model-invocation: true
argument-hint: "[file or claim]"
---

# Ground a document's claims

Target: $ARGUMENTS

If that names a file, read it. If it is a claim written out, work on that. If it is
empty, use the document currently in context, and ask which one only if that is
genuinely ambiguous.

## What to do

1. **Extract the empirical claims.** Statements that assert something checkable about
   the world: an effect, a magnitude, a mechanism, a comparison, a rate. Skip
   definitions, opinions, and statements of intent. Aim for the load-bearing ones
   rather than every sentence; a claim is load-bearing if the argument fails when it
   fails. List them for the user before you start checking, numbered.

2. **Work out the era.** For each claim, decide the year it should be judged against.
   Usually that is the publication year of the source it rests on, or the year the
   document is about. Pass it as `as_of`. This matters more than it sounds: unfenced
   retrieval on an older claim buries the primary report under everything published
   since.

3. **Check each claim.** `zetesis_scope`, then `zetesis_evidence` with short
   entity-anchored queries, then grade against what came back. Work through them in
   order and report as you go rather than saving everything for the end. If a claim
   concerns funding, competitive position, or a company's standing, append the
   entity-only terms to `queries` and set `include_capital: true`; it is off by default
   and without it no grant or filing id can appear.

4. **Report per claim**, briefly:
   - the claim, quoted or tightly paraphrased
   - whether the retrieved evidence supports it, partly supports it, or does not reach it
   - the hard ids that bear on it
   - if it fails, what the evidence actually says instead

## Rules

Cite only ids that came back in a bundle. If nothing relevant was retrieved for a
claim, say exactly that rather than filling the gap from memory; "no retrieved evidence
bears on this" is a legitimate and useful result.

Do not rewrite the user's document unless they ask. The output is an assessment.

Close with the claims that need the author's attention first, ordered by how much the
argument depends on them.
