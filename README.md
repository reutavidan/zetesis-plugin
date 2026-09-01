# Zetesis

**Scientific due diligence inside your assistant.**

Give Zetesis a claim, a paper, an abstract or a deck. It routes the claim to its class,
then returns the questions a domain reviewer would ask, the failure patterns that caught
comparable claims before, and the public evidence bearing on it, with a hard identifier on
every source. Every identifier it hands back was retrieved. None are generated.

It can also evaluate a claim **as it stood in an earlier year**, restricting evidence to
what existed by then, so a claim is judged on what was knowable at the time rather than on
how it turned out.

## Install

```bash
/plugin marketplace add reutavidan/zetesis-plugin
/plugin install zetesis@zetesis
```

Or point Claude Code at a local checkout while developing:

```bash
claude --plugin-dir ./zetesis-plugin
```

No account, key or token is required. The plugin connects to the hosted server at
`https://api.zetesis.science/mcp`.

## Not using Claude?

Zetesis is a Model Context Protocol server, so the same endpoint works in any MCP client.

| Client | How |
|---|---|
| **Microsoft Copilot Studio** | Tools → Add a tool → Model Context Protocol → server URL, auth **None** |
| **ChatGPT** | Settings → Connectors → Developer mode → add `https://api.zetesis.science/mcp` |
| **Gemini CLI** | `gemini mcp add --transport http zetesis https://api.zetesis.science/mcp` |

For Gemini's `settings.json`, use `httpUrl` rather than `url`; the latter is SSE and will
not connect. Full setup notes: <https://api.zetesis.science/docs>

## What it does

**`zetesis_scope`** routes the claim and returns the diligence apparatus for its class:

- **314 questions across 11 life-science claim classes**, structured by substrate,
  methods, cohort and risk-of-bias. Genomics and MR, single-cell, bulk omics, CRISPR
  screens, clinical trials, real-world evidence, AI clinical decision support, diagnostics,
  preclinical models, cell and gene therapy, structural biology.
- **A nine-pattern failure taxonomy** drawn from studied platform collapses, each carrying
  the companies it was derived from.
- **The edge cases where those patterns were wrong** — companies that carried the signature
  and succeeded anyway. A checklist that only ever fires positive teaches over-rejection,
  so they ship alongside it.

**`zetesis_evidence`** runs the searches and returns a deduplicated bundle from Europe PMC,
ClinicalTrials.gov, openFDA, NIH RePORTER and SEC EDGAR, every source carrying a PMID, DOI,
NCT number, grant number or filing reference, followed by the grading rubric.

**Neither of those calls a language model.** They return in under a second, cost nothing to
run, and send nothing to a model provider. That is usually the answer a security reviewer
is looking for.

**`evaluate_claim`** produces Zetesis's own graded reading server-side. Slower, and only
needed when the assessment itself is the deliverable rather than the evidence.

**`verify_attestation`** re-checks a signed Zetesis record to confirm its claim, evidence
and conclusion have not been altered since signing.

## Skills

`evidence-check` is model-invoked: Claude reaches for it when an empirical scientific claim
needs grounding rather than an answer from memory. The other three read local files, so
they never fire on their own and are yours to call.

| Skill | Use |
|---|---|
| `/zetesis:ground` | Check the empirical claims in a document and report which hold, which are thin, and which have no support. |
| `/zetesis:cite-check` | Check whether each citation actually supports the sentence it is attached to, and flag identifiers that do not resolve. |
| `/zetesis:as-of` | Reconstruct what the published evidence on a topic contained in a given year. |

## Why the year fence matters

Ask a general model about a 2020 claim today and it answers with years of hindsight; the
publication that mattered at the time is buried under everything published since.

Measured on a control claim: unfenced retrieval **missed the pivotal publication entirely**
and scored 35% evidence coverage. Fenced to the claim's own year, the same query set
retrieved it and coverage rose to 79%.

So the fence is not only about honesty in retrospect. It is a retrieval-precision feature.

## Try it

```
What did the published evidence actually support about aducanumab and cognitive
decline at the end of 2019, using only sources available by then?
```

Then ask the same question without the year and compare. The difference is the point.

## Privacy

The evidence tools send nothing to a model provider. `evaluate_claim` processes content
through a model sub-processor, named along with retention terms and hosting region in the
[privacy policy](https://api.zetesis.science/privacy).

## Licence

MIT. Questions: <hello@zetesis.science>
