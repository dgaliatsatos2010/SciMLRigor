# SciMLRigor 1.2.0 — Validation Report

Date: 2026-09-14

## Scope

Version 1.2.0 extends the forensic design introduced for R01 across R02–R10 while preserving the canonical R01–R10 identifiers and the 1.x public API.

## Automated validation

- Unit/integration tests: **37/37 passed** with no pytest warnings in the final validation run.
- Legacy v1.1 tests were retained and passed after the upgrade.
- New v1.2 tests cover R02–R10 forensic metadata, regression support, time-aware structured permutation, named feature-group ablations, near-duplicate detection, and report-level forensic helpers.
- Python bytecode compilation: passed.
- Wheel build: passed.
- Source distribution build: passed.
- Wheel installation into an isolated target directory: passed.
- CLI smoke test: `SciMLRigor 1.2.0`.
- Classification smoke audit: passed.
- Regression/time-series smoke audit: passed.

## New forensic coverage

| Test | v1.2 additions |
|---|---|
| R02 | Nested selection under random/group/temporal design; parameter stability; outer-fold dispersion; selection-scope provenance |
| R03 | Group-size imbalance; target heterogeneity; leave-one-group-out worst-group diagnostics; regression support |
| R04 | Temporal embargo; expanding-vs-rolling sensitivity; temporal score trend; target/covariate drift; timestamp-order audit |
| R05 | Exact duplicates; conflicting-target duplicates; near-duplicate nearest-neighbor audit; target discordance |
| R06 | Global permutation plus group-preserving and contiguous-block temporal permutation nulls |
| R07 | Joint and per-field nuisance-only models; high-cardinality IDs; target-pure repeated IDs; regression support |
| R08 | Joint proxy ablation; single-feature ablation; named feature-group ablation with same-design comparison |
| R09 | Macro/weighted domain performance; worst domain; domain score heterogeneity; covariate shift; pairwise domain transfer |
| R10 | Binary Brier/log-loss/ECE/slope/intercept; regression RMSE/MAE/bias/slope/intercept; bootstrap uncertainty |

## Metric-scale safety

Forensic severity thresholds for EOG-producing tests no longer rely on AUC-sized raw score differences. Version 1.2 preferentially uses null-centered Rigor Survival and otherwise uses relative score loss. This prevents a raw difference such as 1.0 RMSE unit from being treated as equivalent to a 1.0 AUC difference.

## Real time-series integration check

The upgraded R04, R05, R06, R08 and R10 modules were executed on the Thessaloniki monthly rainfall data using a leakage-safe regression integration setup. This was a software validation run, not a replacement for the manuscript's exact model pipeline.

Observed integration behavior:

- R04 completed and identified moderate temporal sensitivity.
- R05 completed and found no exact/configured near-duplicate contamination in the tested matrix.
- R06 completed and executed the temporal block-preserving permutation challenge.
- R08 completed named scientific feature-group ablations rather than failing when the named groups collectively covered all predictors.
- R10 completed regression reliability evaluation and returned calibration-slope/bias diagnostics.

The machine-readable audit is saved as `SciMLRigor-1.2.0-rainfall-forensic-validation.json`.

## Compatibility

- Existing `audit(...)` and `RigorAudit` workflows remain valid.
- Canonical test codes R01–R10 are unchanged.
- Existing R01 leakage APIs are unchanged.
- Legacy R10 binary-classification metadata keys are retained.
- New arguments are optional.

## Interpretation constraint

SciMLRigor findings are methodological sensitivity diagnostics. A `critical`, `high`, or `moderate` finding is not an automatic statement that a manuscript is invalid; the scientific meaning depends on the intended estimand, prediction time, sampling design, and domain context.
