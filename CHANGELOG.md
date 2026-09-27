# Changelog

## 1.5.2 — 2026-09-15

### Added
- First-class SciMLRigorBench evidence grading with tiers A/B/C/P.
- `tag_evidence(...)` helper for explicit benchmark provenance without mutating datasets.
- Machine-readable evidence tier, provider, source status, reference and evidence note fields in `ExternalBenchmark` results.
- Evidence-tier counts in external benchmark summaries.

### Rationale
- Prevents protocol/fixture validation from being confused with authoritative source-backed external validation.
- Makes the Bench-to-evidence relationship directly inspectable in CSV/JSON outputs and publication supplements.


## 1.5.1 — 2026-09-15

### Added
- SciMLRigorBench v3 external protocols for UCI Bike Sharing and ACS/Folktables ACSIncome.
- Independent Bike Sharing target-identity verification and safe/forensic benchmark arms.
- Canonical Folktables ACSIncome CSV adapter plus provenance-aware preparation script.
- `TemporalBlockForwardSplit` for repeated/coarse global timestamps such as survey year.
- Geographic and temporal transfer diagnostics for ACS/Folktables.
- Machine-readable benchmark-to-engine provenance fields recording the SciMLRigor version and `scimlrigor.api.audit` entry point.
- Live-engine version resolution for benchmark provenance, preventing stale installed-distribution metadata from being reported during source-tree validation.

### Changed
- R04 automatically uses time-block-aware forward validation when global timestamps repeat, preventing one time block from being split between train and test.
- ARCENE v3 scope is R02-only; R08 remains deferred because probe identities are not independently recoverable.
- C-MAPSS R09 remains deferred until an explicit operating-condition domain definition is independently justified.

### Validation
- 76/76 automated tests pass.
- Bike Sharing fixture protocol: 5/5 runs completed, 0 errors, curated forensic R01 expectation met.
- ACS fixture protocols: geographic R09 and repeated-year R04 completed with 0 errors; R04 used intact time blocks.
- Controlled 60-run benchmark rerun: injection recall 1.000, detection recall 1.000, localization 1.000, mean severity monotonicity 0.9598076211, clean-control alert rate 0.25.
- Rainfall, UCI Hydraulic, and UCI AI4I source-backed anchors rerun under 1.5.1: 10/10 completed, 0 errors, 0 NOT_EVALUABLE, curated expectation agreement 2/2.
- Added machine-readable v3 evidence tiers so protocol/fixture validation cannot be confused with source-backed external execution.

## 1.5.0 — 2026-09-15

### Added
- Conservative multi-feature target-reconstruction forensics in R01 for deterministic targets assembled from predictor pairs.
- Group-aware target lag/lead/rolling formula probes in `leakage_scan(..., groups=..., time=...)`.
- `GroupedTemporalForwardSplit` for panel/longitudinal local clocks.
- Grouped-temporal outer and inner nesting in R02.
- External benchmark loaders for UCI Bike Sharing, UCI HAR Smartphones, ARCENE, and NASA C-MAPSS.
- Per-dataset/per-R-test estimator, scoring, parameter-grid, and audit-context overrides in `ExternalBenchmark`.
- SciMLRigorBench v3 anchor-readiness matrix and benchmark-driven hardening protocol.

### Fixed
- R04 no longer globally interleaves panel units whose local time axes restart within group.
- R02 temporal nested selection now follows the same group-aware chronology as panel R04.
- Restored the UCI Heart multicenter benchmark helper omitted from the extracted 1.4 source tree and added it to the source manifest.

### Validation
- 59/59 automated tests pass.
- Controlled 60-run benchmark remains stable: injection recall 1.000, localization 1.000, severity monotonicity 0.9598.
- Source-backed external anchors rerun: 10/10 completed, 0 errors, curated structural expectation agreement 2/2.
- HAR, ARCENE, C-MAPSS, and Bike Sharing loaders are fixture-validated/anchor-ready; full source execution is not claimed in this release.

## 1.4.0 — 2026-09-15

- Added `ExternalBenchmark` for natural/external dataset anchors without treating every natural alert as a binary ground-truth failure.
- Added rainfall, hydraulic-system, and AI4I external loaders with explicit time/domain/identifier/engineered-feature metadata.
- Added curated expectation agreement with subtest localization.
- Added SciMLRigorBench v2 external-anchor outputs and validation documentation.
- Re-ran the 60-run controlled benchmark unchanged and expanded automated tests to 46.

## 1.3.0 — 2026-09-15

### Added
- `scimlrigor.bench` with `Benchmark`, `BenchmarkRun`, `BenchmarkDataset`, dataset loaders, controlled R01–R10 failure injections, paired-control evaluation, and machine-readable benchmark summaries.
- Network-free real-world anchors: Wisconsin breast cancer, diabetes regression, digits, and wine.
- Explicit OpenML, local CSV, and array adapters for external benchmark datasets without import-time downloads.
- Paired benchmark metrics for injection recall, localization, diagnostic deltas, clean-control alerting, and severity monotonicity.
- CLI command `scimlrigor bench`.
- Reproducible SciMLRigorBench v1 protocol and run-level outputs under `benchmarks/scimlrigorbench_v1/`.
- Failure injection families for target leakage, selection scope, group shortcuts, temporal concept drift, duplicates, label-signal degradation, identifier shortcuts, target proxies, domain concept shift, and calibration distortion.

### Fixed
- High-cardinality numeric targets with integer-valued observations are no longer automatically treated as multiclass; regression-like cardinality is handled conservatively as regression.
- Benchmark severity comparisons now use paired evaluation seeds so severity response is not confounded by changing folds/model randomness.
- R06 benchmark uses a minimum of 19 permutations for a meaningful empirical-null stress.
- R10 benchmark distortion now gives controlled monotone reliability degradation for both classification and regression anchors.

### Validation
- 42/42 automated tests pass in the release worktree.
- Initial benchmark-hardening protocol: 60 runs, 38 expected injected positives, 100% paired injection recall, 100% localization, no execution errors, and mean severity monotonicity ≈0.960.
- Clean-control alert rate is reported separately and must not be interpreted as a proven false-positive rate on heterogeneous real-world controls.

### Compatibility
- Existing 1.x `audit(...)`, `RigorAudit`, R01–R10 identifiers, and result fields remain backward compatible.
- Benchmark APIs are additive.



## 1.2.0 — 2026-09-14

### Added
- R02 selection/tuning forensics: dependence-aware nested evaluation, outer-fold parameter stability, score dispersion, and `selection_scope` provenance.
- R03 group forensics: group-size imbalance, outcome heterogeneity, leave-one-group-out worst-group diagnostics, and regression support.
- R04 temporal forensics: embargo gaps, rolling-window sensitivity, time-order checks, fold-trend analysis, target drift, and numeric covariate drift.
- R05 near-duplicate forensics in addition to exact duplicates, including conflicting-target and near-duplicate discordance diagnostics.
- R06 structure-preserving permutation nulls for grouped and temporal data, with empirical-null separation statistics.
- R07 joint and per-identifier shortcut challenges, high-cardinality warnings, repeated target-pure identifier checks, and regression support.
- R08 per-feature and named feature-group ablations using the same evaluation design for the full and ablated models.
- R09 domain-specific sizes/outcome summaries, covariate-shift diagnostics, worst-domain analysis, and pairwise domain transfer matrices.
- R10 binary calibration diagnostics (Brier, log loss, ECE, calibration slope/intercept) and regression reliability diagnostics (RMSE, MAE, bias, calibration slope/intercept), with bootstrap uncertainty.
- Adaptive classification/regression CV selection for the baseline/null layer.
- `AuditReport.forensic_findings()`, `severe_forensic_findings()`, and `forensic_profile()`.

### Compatibility
- Canonical R01–R10 codes and existing core result fields remain unchanged.
- All new configuration arguments are optional. Existing 1.0/1.1 calls remain valid.
- Legacy R10 classification metadata keys (`delta_brier`, calibration slope/intercept fields) are preserved.


## 1.1.0 — 2026-09-14

### Added
- Standalone `leakage_scan(...)` public API for feature-level outcome-leakage forensics.
- R01a preprocessing-isolation subtest (backward-compatible with the original R01 score fields).
- R01b engineered-feature lineage audit with explicit `feature_provenance` support.
- R01c target-derived feature challenge for direct/affine label copies, deterministic target transforms, future-target leads, and common current-target rolling formulas.
- R01d prediction-time availability audit with `prediction_horizon` and `feature_availability`.
- Warnings for engineered features whose provenance is not supplied.
- Seven leakage-specific unit tests, including a normalized current-target-change scenario.

### Changed
- R01 is now selected automatically in `tests="auto"` even when no preprocessor is supplied, because feature-level leakage can occur independently of preprocessing.
- R01 name expanded to `Pipeline Isolation and Leakage Forensics`.
- Automated formula matching is affine-scale invariant so leakage remains detectable after min-max or z-score preprocessing of a precomputed feature.

### Compatibility
- Existing R01 `baseline_score`, `stress_score`, `optimism_gap`, `rigor_survival`, `unsafe_fold_scores`, and `safe_fold_scores` are preserved when a preprocessor is supplied.
- New leakage arguments are optional; existing 1.0 calls remain valid.


## 1.0.0 — 2026-09-13

### Stable release
- Public 1.x API promoted from release candidate to stable.
- R01–R10 identifiers and core result fields frozen under the 1.x compatibility contract.
- Release metadata prepared for GitHub, PyPI, and Zenodo.
- No methodological definition changes from 1.0.0rc2.


## 1.0.0rc2 — 2026-09-13

### Added
- 5,000-resample descriptive bootstrap intervals for Benchmark Suite v2.
- 12-seed real-world EOG repeatability analysis.
- Bootstrap intervals for mean real-world EOG across repeated CV seeds.
- Real-world repeatability figure for the manuscript.
- Manuscript updates integrating uncertainty and repeatability results.

### Changed
- Release candidate version advanced from rc1 to rc2.
- No backward-incompatible public API changes.


## 1.0.0rc1 — 2026-09-13

### Added
- Stable one-call `audit(...)` API.
- Command-line interface with `--version` and `comparators`.
- 1.x API stability contract and release criteria.
- DataFrame-preserving temporal ordering.
- Public API and CLI tests.

### Changed
- Development status advanced from Alpha to Beta.
- Public API is now treated as frozen for release-candidate evaluation.


## 0.5.0 — 2026-09-13

### Added
- Network-free real-world validation suite executed successfully.
- Real-world EOG figure and v0.5 validation-status report.
- Comparator registry for Deepchecks, Evidently, mlinspect, and mlscorecheck.
- Documented-capability matrix separated from empirical comparator claims.
- UCI Heart Disease four-center loader and R09 benchmark runner.
- Real-world validation suite using scikit-learn and statsmodels datasets.
- Comparator benchmark protocol.
- Tests for comparator registry and multicenter UCI parser.


## 0.4.0 — 2026-09-13

### Added
- Fragility ranking and primary-fragility reporting.
- Reproducibility manifest for audit runs.
- Real-world Wisconsin Diagnostic Breast Cancer demonstration.
- Research-positioning and release-checklist documentation.
- Benchmark Suite v2 with factorial robustness validation.
- Variation across sample size, class prevalence, estimator family, severity, and random seed.
- 320 controlled runs across five methodological-failure scenarios.
- Stratified severity-response correlations.
- Directional robustness summaries across experimental strata.


## 0.3.0 — 2026-09-13

### Added
- Benchmark Suite v1 with controlled severity-response experiments.
- Five failure-mode families: duplicates, group dependence, temporal drift, proxy strength, and domain shift.
- Six severity levels and five random seeds per scenario (150 runs).
- Automated aggregation of EOG and RS severity-response curves.
- Spearman severity-response diagnostics.
- Machine-readable benchmark CSV/JSON outputs and Markdown report.


## 0.2.0 — 2026-09-13

### Added
- R07 Identifier Challenge.
- R08 Proxy Ablation.
- R09 Domain Holdout.
- R10 Calibration Survival.
- Auto-selection of R07-R10 when relevant metadata are supplied.
- Wheel build and isolated import smoke-test workflow.

## 0.1.0 — 2026-09-13

Initial research prototype.

### Added
- Baseline stratified cross-validation.
- Empirical permutation-null estimation.
- Evaluation Optimism Gap (EOG).
- Null-centered Rigor Survival (RS).
- R01 Pipeline Isolation.
- R02 Nested Model Selection.
- R03 Group Independence.
- R04 Temporal Generalization.
- R05 Duplicate Robustness.
- R06 Permutation Integrity.
- DataFrame, JSON and Markdown reporting.
- Controlled synthetic benchmark scenarios.
- Automated tests.
