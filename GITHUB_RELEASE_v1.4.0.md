# SciMLRigor 1.4.0 — External-Anchor Validation Release

SciMLRigor 1.4.0 adds a natural/external validation layer on top of the R01–R10 forensic framework and the controlled SciMLRigorBench introduced in 1.3.0.

## Added

- `ExternalBenchmark` and `ExternalBenchmarkRun`.
- `load_rainfall_workbook()` for engineered-feature / temporal regression anchors.
- `load_hydraulic_zip()` for cycle-level hydraulic sensor/domain/time anchors.
- `load_ai4i_csv()` with explicit separation of process predictors, domains, identifiers, and failure-mode target fields.
- Curated expectation scoring that checks both minimum forensic severity and expected subtest localization.
- `benchmarks/scimlrigorbench_v2/` with machine-readable external-anchor outputs and reproducible scripts.

## Validation

- 46/46 automated tests pass.
- The 60-run v1 controlled failure-injection benchmark was rerun with unchanged aggregate results.
- 10 natural external-anchor R-test runs completed with zero errors.
- Both curated structural expectations were detected and localized (2/2).

See `VALIDATION_v1.4.0.md` for exact scope, results, and limitations.
