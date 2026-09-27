# SciMLRigor 1.3.0 — Benchmark-Hardened Release

SciMLRigor 1.3.0 introduces **SciMLRigorBench**, an additive paired-control failure-injection framework for validating the R01–R10 forensic diagnostics on real-world data anchors.

## Highlights

- `scimlrigor.bench.Benchmark` and `BenchmarkDataset`
- controlled severity 0–4 failure injection across R01–R10
- network-free built-in real-world anchors plus OpenML/CSV/array adapters
- paired injection recall and localization metrics
- severity-response diagnostics and monotonicity analysis
- clean-control alert reporting without mislabeling natural real-world fragility as a false positive
- CLI `scimlrigor bench`
- regression target-kind edge-case fix found by benchmark hardening
- 42/42 automated tests passing

The bundled smoke protocol executed 60 runs with 38 a-priori expected positive injections. It achieved 100% paired injection recall and localization in this internal protocol, with mean severity-response monotonicity ≈0.960 and zero execution errors. These figures are **internal benchmark-hardening results**, not universal performance claims.

See `VALIDATION_v1.3.0.md` and `benchmarks/scimlrigorbench_v1/` for the exact protocol, limitations, and machine-readable results.
