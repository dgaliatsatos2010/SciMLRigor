# SciMLRigor 1.5.1 validation report

Date: 2026-09-15

## Purpose

This release extends SciMLRigorBench v3 without adding new canonical R-codes. The benchmark is used to expose methodological blind spots in the existing R01-R10 engine and to build source-ready external protocols while preserving a strict boundary between fixture validation and source-backed external evidence.

## Automated validation

The complete source suite passes **76/76 tests**.

New coverage beyond 1.5.0 includes:

- UCI HAR provenance/split protocol checks;
- NASA C-MAPSS row-vs-group overlap, grouped chronology, RUL, and supported-test scope checks;
- UCI ARCENE selection-scope, feature-stability, and nested R02 execution checks;
- UCI Bike Sharing structural target-identity and safe/forensic R01 protocol checks;
- ACS/Folktables loader, geographic R09, temporal R04, and independent transfer diagnostics;
- `TemporalBlockForwardSplit` tests ensuring equal timestamps never appear in both train and test;
- R04 integration tests for repeated coarse time blocks.

## Benchmark-driven R04 hardening

ACS survey-year benchmarking exposed that a row-wise temporal splitter is insufficient when many observations share the same coarse timestamp. After sorting, the same year could occur in both training and test partitions.

`TemporalBlockForwardSplit` was added for repeated global timestamps. R04 now automatically uses the `global_temporal_blocks` design when duplicate timestamps are present. Forward folds operate on unique ordered time blocks, so all observations from one time block remain together. Drift diagnostics also compare early and late time blocks rather than arbitrary row slices in this mode.

A format-faithful five-year ACS smoke fixture completed R04 with 5 unique time blocks and no execution error.

## UCI Bike Sharing protocol

The independent structural fact is the UCI target definition `cnt = casual + registered`. The benchmark checks this identity directly from source rows.

- Safe arm: excludes `casual` and `registered` from X.
- Forensic arm: retains both target components intentionally.
- Curated expectation: R01 `multifeature_target_reconstruction`.

Fixture smoke result:

- 5/5 runs completed;
- 0 errors;
- curated R01 expectation agreement 1/1;
- safe-arm R04/R09/R10 findings are descriptive only.

## ACS/Folktables protocol

A canonical ACSIncome CSV adapter and preparation script were added. State, year, label, and protected-group metadata are excluded from X.

- Geographic protocol: state-domain R09 plus independent train-state → test-state AUC matrix.
- Temporal protocol: repeated-year R04 plus independent earliest-year → later-year transfer evaluation.
- Source-backed status requires sidecar provenance from `folktables.ACSDataSource/ACSIncome`.

Fixture smoke result:

- geographic R09 completed with 0 errors;
- temporal R04 completed with 0 errors;
- R04 selected `global_temporal_blocks` over five survey-year blocks.

## External-evidence status

The rainfall, UCI Hydraulic, and UCI AI4I anchors were freshly rerun from their retained source snapshots under the 1.5.1 engine. Source SHA-256 values matched the previously validated snapshots exactly. Across 10 external R-test runs: 10 completed, 0 errors, 0 NOT_EVALUABLE, 7 alerts, and 2/2 independently curated structural expectations were met.

HAR, C-MAPSS, ARCENE, Bike Sharing, and ACS/Folktables remain protocol/fixture validated but are not promoted to source-backed external validation. NASA currently states on its Open Data Portal that C-MAPSS and C-MAPSS40K are unavailable for download; SciMLRigor therefore does not substitute an unofficial mirror as source-backed NASA evidence. This distinction is deliberate: parser correctness, benchmark structure, and format-faithful smoke execution are necessary but not sufficient evidence of external validation on the original source data.

## Controlled benchmark regression

Because the ACS protocol changed core R04 splitting behavior for repeated global timestamps, the 60-run controlled SciMLRigorBench v1 protocol was rerun under 1.5.1.

- Runs: **60**
- Expected-positive injected cases: **38**
- Injection recall: **1.000**
- Detection recall: **1.000**
- Localization accuracy: **1.000**
- Mean severity monotonicity: **0.9598076211**
- Clean-control alert rate: **0.25**

These values match the 1.5.0 controlled benchmark results, indicating that the repeated-time R04 hardening did not regress the established controlled-failure behavior.

The three established source-backed external anchors were rerun from retained snapshots after the final hardening changes. Their source hashes were: rainfall `19ce28167227c2915d24f26e4701c3598b9532c02bf36ec4f1f3fab5070e429f`, hydraulic `24128aad2ee45eea7e6b63ebbd9992cdf25d0483a2cebefbfc13bc69079af1f2`, and AI4I `dc6630cd9b1f0f853922fad78a1b6436570d3f1ec863f1dd5c4340ac56bc8a8e`. The combined run completed 10/10 tests with zero errors and 2/2 curated expectations met.

## Packaging smoke validation

A wheel was built locally with build isolation disabled (to avoid network dependency resolution) and installed into a clean target directory. The installed package reports version `1.5.1`, an installed-package `ExternalBenchmark` records `1.5.1` and `scimlrigor.api.audit`, and the new `load_folktables_acs_csv`, existing `load_bike_sharing_csv`, and `TemporalBlockForwardSplit` APIs import successfully. The source distribution was also extracted into a clean directory and its bundled test suite passed 76/76 tests. The external-anchor runner was also made portable: source locations are supplied by CLI flags or environment variables rather than validation-host-specific absolute paths.

## Bench-to-engine provenance

Both controlled `Benchmark` and natural-anchor `ExternalBenchmark` runners are regression-tested to call the same public `scimlrigor.api.audit` engine used by package users. During finalization, this regression test exposed stale installed-distribution metadata being reported during source-tree validation; the version resolver was hardened to prefer the live imported engine version, then fall back to distribution metadata. External benchmark rows and summaries now record `scimlrigor_version`, `engine_entrypoint`, and `benchmark_layer`, making the evaluated core version explicit in machine-readable outputs.

## Evidence tiers

`benchmarks/scimlrigorbench_v3/evidence_status.csv` is the normative release ledger for external evidence. Tier A denotes a source-backed execution under the release engine. Tier B denotes a source-ready protocol validated with format-faithful fixtures but awaiting original source execution. Tier C records a provider-access limitation. Tier D records parser/packaging evidence only. Only Tier A anchors contribute to the release's source-backed external-run count.
