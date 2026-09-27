# SciMLRigor 1.5.0 — Benchmark-Driven Hardening Release

SciMLRigor 1.5.0 strengthens the existing R01–R10 framework using failure modes exposed by domain-specific benchmark design.

## Highlights

- **R01 composite target reconstruction**: detects near-deterministic targets assembled from pairs of predictors (for example, total = component A + component B).
- **Group-aware leakage formulas**: lag/lead/rolling target probes stay within subject/engine boundaries when `groups` and `time` are supplied.
- **GroupedTemporalForwardSplit**: R04 now handles panel/local clocks without globally interleaving subjects or engines.
- **Grouped-temporal R02**: nested outer and inner model-selection folds now respect the same panel chronology.
- **New benchmark loaders**: UCI Bike Sharing, UCI HAR, ARCENE, and NASA C-MAPSS.
- **Configurable ExternalBenchmark**: per-anchor/per-R-test estimator, scoring, parameter-grid and audit overrides.
- **Packaging fix**: restores the UCI Heart multicenter benchmark helper to the distributed source tree.

## Validation

- 59/59 automated tests passed.
- 60-run controlled R01–R10 failure-injection benchmark rerun: recall 1.000, localization 1.000, severity monotonicity 0.9598.
- 10/10 source-backed external-anchor R-test runs completed with zero execution errors.
- 2/2 independently curated structural expectations remained correctly detected/localized.

The new HAR, ARCENE, C-MAPSS, and Bike Sharing loaders are parser/protocol validated but their full source datasets are **not claimed as executed external validation** in this release.

See `VALIDATION_v1.5.0.md` and `benchmarks/scimlrigorbench_v3/` for exact scope and limitations.
