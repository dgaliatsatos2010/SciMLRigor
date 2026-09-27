# SciMLRigor 1.5.0 validation report

Date: 2026-09-15

## Purpose

SciMLRigor 1.5.0 is a benchmark-driven hardening release. It does not add new canonical R-codes. Instead, real-world benchmark design exposed two methodological blind spots inside existing tests: composite target reconstruction in R01 and invalid global temporal ordering for panel/longitudinal data in R02/R04. Both were corrected while preserving the R01–R10 API.

## Software validation

The release worktree passed **59/59 automated tests**. The tests include the previous unit/integration suite plus new checks for:

- two-feature deterministic target reconstruction;
- avoidance of duplicate composite findings when a direct target copy already exists;
- group-aware target lag/formula scanning;
- within-group forward chronology in `GroupedTemporalForwardSplit`;
- grouped-temporal R04 execution;
- grouped-temporal outer and inner nesting for R02;
- UCI Bike Sharing safe/forensic loader modes;
- UCI HAR subject-group parsing;
- ARCENE labeled high-dimensional parsing;
- NASA C-MAPSS engine/cycle/RUL parsing;
- per-dataset/per-R-test external benchmark overrides;
- restoration of the multicenter UCI Heart benchmark parser in the source distribution.

## Controlled failure-injection benchmark

The 60-run SciMLRigorBench v1 protocol was rerun under the 1.5.0 codebase after the core changes.

- Runs: **60**
- Expected-positive injected cases: **38**
- Paired injection recall: **1.000**
- Detection recall: **1.000**
- Localization accuracy: **1.000**
- Mean severity monotonicity: **0.9598076211**
- Clean-control alert rate: **0.25**

The clean-control alert rate is not interpreted as a false-positive rate because unmodified real-world anchors may contain genuine methodological fragility.

## External-anchor revalidation

The three source-backed anchors from SciMLRigor 1.4.0 were rerun under the 1.5.0 codebase:

- Thessaloniki monthly rainfall: R01, R04, R05, R08, R10
- UCI Hydraulic Condition Monitoring: R04, R09, R10
- UCI AI4I 2020: R07, R09

Result:

- External R-test runs: **10/10 completed**
- Execution errors: **0**
- NOT_EVALUABLE: **0**
- Moderate-or-higher alerts: **7**
- Independently curated structural expectations: **2**
- Curated expectation agreement: **2/2 = 1.000**

The curated expectations remain deliberately narrow: the rainfall `Pvar` current-target relationship and AI4I near-unique identifier structure. This is not a universal sensitivity/specificity claim.

## New benchmark anchors prepared in 1.5.0

Four additional domain-specific loaders and protocols were implemented and tested with format-faithful local fixtures:

1. **UCI Bike Sharing** — motivates multi-feature target reconstruction because raw target-component columns can deterministically form the total target. Safe mode excludes these components; forensic mode retains them intentionally.
2. **UCI HAR Smartphones** — subject IDs are parsed as groups and never inserted into X, preparing a natural R03 subject-independence experiment.
3. **UCI ARCENE** — labeled train/validation subsets are parsed for high-dimensional R02/R08 experiments.
4. **NASA C-MAPSS** — engine unit is group, cycle is local time, and training-set RUL is constructed per engine, preparing grouped-temporal R02/R04 experiments.

The original full HAR, ARCENE, C-MAPSS, and Bike Sharing source snapshots were not executed end-to-end in the current network-restricted validation environment. They are therefore labeled **anchor-ready**, not externally validated, in `benchmarks/scimlrigorbench_v3/anchor_matrix.csv`.

## Core methodological changes

### R01 composite-target reconstruction

`leakage_scan` now includes a conservative pairwise affine reconstruction challenge. Candidate numeric features are screened by marginal association, then up to a configurable number are tested in pairs. A CRITICAL finding is emitted only when the pair reconstructs the target essentially deterministically and neither feature alone already satisfies the direct-copy threshold. Default threshold: R² >= 0.999999.

### Group-aware leakage formulas

When both `groups` and `time` are supplied, target lag/lead/rolling formula probes are computed within group. This prevents target history from crossing engine/subject boundaries.

### R04 grouped temporal generalization

`GroupedTemporalForwardSplit` creates expanding or rolling forward blocks independently within each eligible group. R04 automatically activates this design when both groups and time exist. Drift diagnostics are computed using relative early/late portions within groups.

### R02 grouped-temporal nested selection

When grouped longitudinal data are supplied, R02 now uses grouped-temporal splitting in both outer and inner model-selection folds. This closes a mismatch in which nested tuning could otherwise use a global temporal order even though R04 used a panel-aware design.

### External benchmark overrides

`ExternalBenchmark` now accepts per-dataset/per-R-test estimator, scoring, parameter-grid and audit-context overrides. This is required for domain-specific anchors such as ARCENE, where R02 must receive an explicit feature/tuning search space rather than being forced through one generic estimator configuration.

## Packaging regression found during hardening

A full-suite run of the extracted 1.4 source revealed that `tests/test_heart_loader.py` referenced `benchmarks/external_data/heart_disease_uci.py`, but that benchmark helper was absent from the 1.4 release source tree. The parser and runner were restored and the source manifest was updated so the benchmark helper is included in 1.5 distributions. This is a packaging/reproducibility fix, not a change in R09 mathematics.

## Interpretation and limitations

SciMLRigor remains a stress-testing framework rather than a proof that a model or manuscript is valid. Automated leakage detection is intentionally conservative and cannot guarantee absence of arbitrary hidden transformations. Natural external findings are descriptive unless an independent structural expectation was defined before inspecting the SciMLRigor result. Full domain-general sensitivity/specificity estimation will require additional source-backed anchors and version-pinned comparator methods.
