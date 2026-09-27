# SciMLRigor 1.3.0 validation report

## Scope

SciMLRigor 1.3.0 adds **SciMLRigorBench**, a paired-control failure-injection framework designed to harden the canonical R01–R10 forensic diagnostics against known methodological failures. The benchmark is a validation layer for the software; it is not a paper-quality score and it is not a claim of universal scientific validity.

## Release checks

- Package version: `1.3.0`
- Automated tests: **42/42 passed**
- Network-free benchmark anchors: Wisconsin Diagnostic Breast Cancer (classification) and scikit-learn Diabetes (regression)
- R-tests: R01–R10
- Injection severities: 0, 2, 4
- Paired evaluation seed: fixed within each dataset × R-test pair
- Cross-validation: 2 folds for the smoke-validation protocol
- R06 permutation minimum: 19
- Maximum observations per anchor in the smoke protocol: 140
- Benchmark execution errors: **0**

## Failure-injection families

| Code | Controlled failure |
|---|---|
| R01 | target-derived engineered feature with progressively decreasing noise |
| R02 | selection/tuning scope moved from fold-local to increasingly unsafe global scope |
| R03 | repeated groups with a group shortcut and decreasing within-group noise |
| R04 | time-varying shortcut plus progressively stronger concept drift |
| R05 | increasing exact duplicate contamination |
| R06 | progressive destruction of label signal by permutation |
| R07 | increasingly target-informative identifier/nuisance variable |
| R08 | target-adjacent proxy plus weakening of the original feature–outcome relation |
| R09 | domain-conditional shortcut plus increasing domain concept shift |
| R10 | controlled classification probability collapse or regression scale/bias distortion |

## Initial benchmark-hardening results

The bundled protocol contains **60 runs**, including **20 severity-0 real-world controls** and **38 runs designated a priori as expected positive injections**. Across these expected positives:

- paired injection recall: **1.000**
- localization accuracy: **1.000**
- mean severity-response monotonicity: **0.9598**
- execution errors: **0**
- clean-control alert rate: **0.250**

The clean-control alert rate must not be read as a false-positive rate. Severity-0 controls are unmodified real-world datasets, and SciMLRigor can legitimately detect pre-existing group, temporal, domain, or calibration fragility. In this smoke protocol, such baseline alerts occurred mainly in R03/R04/R09 for the diabetes anchor and in R10 for both anchors. The paired benchmark therefore asks whether the known injection produces a stronger diagnostic response than the same dataset before injection.

## Per-code results

| Code | Injection recall | Localization | Clean-control alert | Severity monotonicity |
|---|---:|---:|---:|---:|
| R01 | 1.000 | 1.000 | 0.000 | 1.000 |
| R02 | 1.000 | 1.000 | 0.000 | 0.866 |
| R03 | 1.000 | 1.000 | 0.500 | 1.000 |
| R04 | 1.000 | 1.000 | 0.500 | 1.000 |
| R05 | 1.000 | 1.000 | 0.000 | 1.000 |
| R06 | 1.000 | 1.000 | 0.000 | 0.866 |
| R07 | 1.000 | 1.000 | 0.000 | 0.933 |
| R08 | 1.000 | 1.000 | 0.000 | 0.933 |
| R09 | 1.000 | 1.000 | 0.500 | 1.000 |
| R10 | 1.000 | 1.000 | 1.000 | 1.000 |

## Software defect discovered by benchmark hardening

During benchmark execution, a high-cardinality integer-valued regression outcome exposed an edge case in target-type inference: numeric regression targets with integer-valued observations could be interpreted as multiclass. SciMLRigor 1.3.0 fixes this by treating high-cardinality numeric targets conservatively as regression while retaining normal low-cardinality multiclass behavior. A regression-specific unit test was added.

This is an example of why SciMLRigorBench is useful beyond demonstrating detector performance: the benchmark also stress-tests SciMLRigor itself.

## Interpretation and limitations

These results demonstrate that the current controlled injections are detected and localized under the bundled two-anchor smoke protocol. They do **not** demonstrate universal sensitivity/specificity across scientific domains, estimators, sample sizes, feature geometries, or naturally occurring failure mechanisms. The benchmark uses deliberately constructed failure families, so publication-grade validation must include independent real-world anchors with natural group, temporal, and domain structure and additional pristine synthetic controls for a genuine false-positive-rate estimate.

Planned external anchors include UCI HAR, NASA C-MAPSS, UCI Bike Sharing, Appliances Energy Prediction, Folktables/ACSIncome, ARCENE, and broader OpenML classification/regression suites. They are documented in `benchmarks/scimlrigorbench_v1/EXTERNAL_ANCHORS.md` but are **not claimed as executed validation in 1.3.0**.

## Reproduction

From the repository root:

```bash
PYTHONPATH=src pytest -q
PYTHONPATH=src python benchmarks/scimlrigorbench_v1/run_benchmark.py
```

Machine-readable outputs:

- `benchmarks/scimlrigorbench_v1/benchmark_runs.csv`
- `benchmarks/scimlrigorbench_v1/benchmark_by_code.csv`
- `benchmarks/scimlrigorbench_v1/benchmark_summary.csv`
