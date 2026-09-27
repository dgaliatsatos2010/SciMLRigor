# SciMLRigor 1.5.2

SciMLRigor 1.5.2 is an evidence-provenance hardening release for SciMLRigorBench. It keeps the R01-R10 scientific logic unchanged while making the strength of external benchmark evidence machine-readable.

## Highlights
- Adds evidence tiers **A/B/C/P** to external benchmark provenance.
- Adds `tag_evidence(...)` for explicit non-mutating provenance tagging.
- `ExternalBenchmark` rows now expose evidence tier, provider, source status, source reference and evidence note.
- External summaries now count runs by evidence tier.
- Primary external claims are restricted to Tier A source-backed execution.
- Controlled 60-run benchmark remains unchanged: recall 1.000, localization 1.000, severity monotonicity 0.9598076211.
- Tier-A external layer rerun under 1.5.2: 10/10 completed, 0 errors, 0 NOT_EVALUABLE, curated expectation agreement 2/2.
- Automated test suite: **79/79 passed**.

See `VALIDATION_v1.5.2.md` and `benchmarks/scimlrigorbench_v3/EVIDENCE_MODEL.md`.
