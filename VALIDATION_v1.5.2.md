# SciMLRigor 1.5.2 validation report

Date: 2026-09-15

## Release purpose

SciMLRigor 1.5.2 adds first-class evidence grading to SciMLRigorBench. The change does not introduce new R-codes or alter the scientific meaning of R01-R10. It prevents source-backed external validation, verified-mirror execution, fixture/protocol validation and pending-source protocols from being conflated in machine-readable outputs.

## Software validation

- Automated tests: **79/79 passed**.
- The public package version is **1.5.2**.
- `ExternalBenchmark` continues to call the released core engine (`scimlrigor.api.audit`) rather than duplicate R-code logic.

## Controlled failure-injection benchmark

The 60-run paired controlled benchmark was rerun under 1.5.2.

- Runs: **60**
- Expected-positive cases: **38**
- Injection recall: **1.000**
- Detection recall: **1.000**
- Localization accuracy: **1.000**
- Mean severity monotonicity: **0.9598076211**
- Clean-control alert rate: **0.25**

The clean-control alert rate is not treated as a false-positive rate because natural controls may contain genuine methodological fragility.

## Tier-A source-backed external validation

The authoritative/source-backed layer was rerun end-to-end under 1.5.2 and is now explicitly tagged Tier A in each result row.

- Datasets: **3**
- R-test executions: **10**
- Completed: **10/10**
- Errors: **0**
- NOT_EVALUABLE: **0**
- Alerts: **7**
- Independently curated structural expectations: **2**
- Curated expectation agreement: **2/2 = 1.000**
- Tier-A result rows: **10/10**

The two curated expectations remain deliberately narrow: the Thessaloniki rainfall `Pvar` current-target relationship and the AI4I near-unique identifier structure. This is not a universal sensitivity/specificity estimate.

## Evidence tiers

- **A** — authoritative/original source snapshot executed end-to-end.
- **B** — verified mirror executed and independently cross-checked against an authoritative source.
- **C** — protocol/fixture/parser validation only; not external validation.
- **P** — protocol prepared but source execution pending or provider unavailable.

Only Tier A is included in the primary source-backed external-validation summary. Tier B, if later added, must be reported separately. Tier C/P may support implementation/readiness claims only.

## Pending anchors

UCI HAR, UCI Bike Sharing, UCI ARCENE and ACS/Folktables remain Tier C until their source snapshots are executed in the benchmark environment. NASA C-MAPSS is Tier P because the authoritative provider currently does not expose a downloadable source snapshot. UCI Heart is parser-validated but is not part of the current primary external layer.

## Interpretation

The defensible claim for 1.5.2 is that SciMLRigor preserves its controlled failure-injection behavior after benchmark hardening and has an initial source-backed external layer with perfect agreement on two narrowly curated structural expectations. The release does not establish universal diagnostic sensitivity, specificity, or domain-general validity.
