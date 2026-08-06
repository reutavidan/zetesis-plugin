---
description: Reconstruct what the published evidence on a topic actually contained in a given year, using only sources available by then.
disable-model-invocation: true
argument-hint: "<claim or topic> <year>"
---

# What the evidence said, as of a year

Request: $ARGUMENTS — expected to name a claim or topic and a year. If the year is
missing, ask for it; it is the whole point of this skill.

## What to do

Run `zetesis_scope` and then `zetesis_evidence` with `as_of` set to that year. Set
`include_capital: true` as well if the topic involves who was funding or filing on it;
that pass is off by default, so NIH grant and SEC filing records are otherwise absent. Retrieval
is fenced to sources published, registered, or filed on or before 31 December of it, and
two fields that leak later outcomes are suppressed: a trial's present-day status, and
FDA labels that took effect after the cutoff.

Then answer as a reader at the end of that year could have, from the returned bundle:
what was established, what was suggestive, what was contested, and what had simply not
been done yet. Distinguish those four clearly. Say plainly which questions were open.

## The discipline that makes this worth anything

You know how this turned out. Set that aside. The output is a reconstruction of the
evidence base, not a verdict informed by hindsight, and it is worthless if hindsight
leaks in.

Concretely: do not write that a result "was later overturned", "would go on to fail", or
"we now know". If the user asks what happened next, answer separately and label it
clearly as outside the reconstruction.

Judge derivation quality, not outcome. A well-derived claim can fail later and a badly
derived one can succeed, so "the evidence at the time reasonably supported this" and
"this turned out to be true" are different findings and should not be conflated.

## Reporting

Lead with what a careful reader would have concluded that year, in one or two sentences.
Then the evidence by hard id, oldest to newest. Then the open questions.

Note the cutoff explicitly in the output, and say how many later sources the fence
withheld if the bundle reports it, so the reader knows the boundary was real.
