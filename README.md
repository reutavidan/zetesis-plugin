# Zetesis for Claude Code

Ground scientific claims in real, dated, citable evidence.

Zetesis retrieves primary literature, clinical trial records, regulatory labels, grants
and filings, returns each source by hard identifier, and grades how well a claim is
actually supported. Every identifier it hands back was retrieved. None are generated.

It can also evaluate a claim **as it stood in an earlier year**, restricting evidence to
what existed by then, which lets you judge how a claim was derived at the time rather
than how it turned out.

## Install

```bash
/plugin marketplace add anthropics/claude-plugins-community
/plugin install zetesis@claude-community
```

Or point Claude Code at a local checkout while developing:

```bash
claude --plugin-dir ./zetesis-plugin
```

The plugin connects to the hosted Zetesis server at `https://api.zetesis.science/mcp`.
No account, key, or token is required for the free tier.

## What you get

Four skills. `evidence-check` is invoked by Claude automatically when it is relevant. The
other three are yours to call: they read local files and send their contents to the hosted
API, so they never fire on their own.

| Skill | Use |
|---|---|
| `evidence-check` | The one Claude reaches for on its own, when an empirical scientific claim needs grounding rather than an answer from memory. |
| `/zetesis:ground` | Check the empirical claims in a document and report which hold. |
| `/zetesis:cite-check` | Check whether each citation actually supports the sentence it is attached to. |
| `/zetesis:as-of` | Reconstruct what the evidence on a topic contained in a given year. |

## Tools

| Tool | Does |
|---|---|
| `zetesis_scope` | Returns the source catalog and scoping rubric so you work out the dimensions and queries yourself. Runs no model. |
| `zetesis_evidence` | Runs the retrieval and returns the sources plus the grading rubric. Takes `queries`, an optional `as_of` year fence, and `include_capital` (default false) which adds the NIH RePORTER and SEC EDGAR pass. Runs no model. |
| `evaluate_claim` | One-shot screen graded on the Zetesis side. |
| `verify_attestation` | Re-checks a signed Zetesis attestation. Public, no credentials. |

`zetesis_scope` and `zetesis_evidence` do no inference at all: they hand you evidence and
a rubric, and your own session does the reasoning. That is why the free tier has no
account behind it.

## Set `as_of` for anything that is not current

Retrieval is live, so a claim from an earlier year competes against everything published
since and relevance ranking buries the original report. Passing the year fixes it. On the
BNT162b2 efficacy claim, unfenced retrieval misses the pivotal trial report entirely and
the claim reads as unsupported; fenced to 2020 the same query returns it and the claim
reads as supported.

The skills infer the year and set it for you. If you call the tools directly, set it
yourself.

## Sources

Europe PMC, ClinicalTrials.gov and openFDA are retrieved by default. NIH RePORTER and
SEC EDGAR are retrieved when `include_capital` is set. All public, all keyless, every
record returned with a resolvable identifier.

## Attestations

A full Zetesis evaluation is signed with ed25519 and pinned to the evidence it used, so a
third party can re-check that the claim, sources and conclusion were not altered after
signing. `verify_attestation` does that check and needs no credentials. Signed dossiers
are available by request at <https://api.zetesis.science/request-access>.

## Privacy

Claims you submit are sent to the hosted Zetesis API to run retrieval and grading. See
<https://api.zetesis.science/privacy>.

## License

MIT
