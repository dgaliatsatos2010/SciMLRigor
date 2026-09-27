# SciMLRigor 1.5.1

SciMLRigor 1.5.1 is a benchmark-driven hardening release focused on repeated/coarse temporal blocks, stronger SciMLRigorBench v3 protocols, and auditable separation of source-backed external evidence from protocol-only validation.

## Highlights

- adds `TemporalBlockForwardSplit` so repeated global timestamps such as survey years remain intact across forward-validation folds;
- finalizes source-ready protocols for UCI HAR, ARCENE, Bike Sharing, NASA C-MAPSS, and ACS/Folktables;
- keeps ARCENE focused on R02 because probe identities are withheld, and keeps C-MAPSS R09 deferred until an independently justified domain definition exists;
- records `scimlrigor_version`, `engine_entrypoint`, and `benchmark_layer` in benchmark outputs;
- adds `benchmarks/scimlrigorbench_v3/evidence_status.csv` so evidence tiers are machine-readable.

## Validation

- 76/76 automated tests pass;
- 60 controlled benchmark runs: injection recall 1.000, detection recall 1.000, localization 1.000, severity monotonicity 0.9598076211;
- 10/10 source-backed external R-test runs completed under the 1.5.1 engine with 0 errors and 2/2 curated structural expectations met.

The release does **not** claim source-backed execution for HAR, ARCENE, Bike Sharing, C-MAPSS, or ACS/Folktables. Their status is recorded explicitly in the evidence ledger. NASA currently lists C-MAPSS as unavailable for download, so no unofficial mirror is promoted as NASA source-backed evidence.
