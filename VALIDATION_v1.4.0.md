# SciMLRigor 1.4.0 validation report

## Scope

SciMLRigor 1.4.0 extends the 1.3.0 controlled failure-injection benchmark with a second validation layer: **natural external anchors**. The goal is to test R01–R10 on datasets that contain real temporal, domain, identifier, engineered-feature, or reliability structure without pretending that every natural finding has a universal binary ground truth.

## Software tests

- Automated test suite: **46/46 passed**.
- The v1.3.0 controlled benchmark was rerun unchanged under the 1.4.0 codebase.
- Controlled benchmark: **60 runs**, **38 a-priori expected-positive injections**, injection recall **1.000**, localization accuracy **1.000**, mean severity monotonicity **0.9598**, clean-control alert rate **0.25**.

The clean-control alert rate is intentionally not called a false-positive rate because real-world severity-0 datasets may contain genuine fragility.

## External-anchor layer

Three anchor datasets were executed in separate runs and combined into one machine-readable table.

| Anchor | Nature | R-tests executed |
|---|---|---|
| Thessaloniki monthly rainfall | real hydroclimatic time series | R01, R04, R05, R08, R10 |
| UCI hydraulic condition monitoring | experimentally obtained test-rig time series | R04, R09, R10 |
| UCI AI4I 2020 | synthetic but industry-realistic predictive maintenance | R07, R09 |

Combined external-anchor result: **10/10 completed**, **0 errors**, **0 NOT_EVALUABLE**, **7 moderate-or-higher alerts**. Each external result row also records the SHA-256 hash of the local source file used for the run, enabling exact source-snapshot provenance checks without redistributing the data. Two structural expectations were curated independently of model performance and both were met/localized (**2/2, agreement 1.000**):

1. Rainfall R01: the engineered `Pvar` column contains current-target information. SciMLRigor localized `current_target_formula` and assigned **critical** severity.
2. AI4I R07: `UDI`/`Product ID` are near-unique identifiers. SciMLRigor localized **R07d** and assigned **warning** severity.

These two checks are deliberately narrow structural expectations; they are not estimates of global benchmark sensitivity/specificity.

## Natural findings (descriptive, not ground-truth labels)

### Rainfall

- R01: **critical**, current-target formula detected for the engineered feature.
- R04: **moderate**, later-fold temporal performance trend flagged.
- R05: no moderate-or-higher duplicate finding.
- R08: **high**, removing the designated proxy/leaky feature produced a large performance drop.
- R10: **moderate**, stricter temporal regression reliability showed material degradation/bias.

### Hydraulic condition monitoring

- R04: **moderate** predictor drift (R04e), with essentially unchanged mean forward discrimination in this configuration.
- R09: **moderate** covariate shift (R09d); the macro domain performance gap was small in this configuration.
- R10: **high** reliability/calibration concern under stricter temporal evaluation.

### AI4I 2020

- R07: **warning** for near-unique identifier metadata (R07d); this is the curated structural expectation.
- R09: no moderate-or-higher domain transport alert across product Types in this configuration.

## New public benchmark API

```python
from scimlrigor.bench import (
    ExternalBenchmark,
    load_rainfall_workbook,
    load_hydraulic_zip,
    load_ai4i_csv,
)
```

`ExternalBenchmark` scores only explicitly curated expectations. Other external findings remain descriptive. This prevents natural real-world fragility from being mislabeled as a false positive.

## Limitations

- The external-anchor layer remains small and should not be interpreted as universal validation.
- The rainfall anchor is project-specific rather than a public benchmark redistributed by SciMLRigor.
- AI4I is synthetic, although designed to reflect industrial predictive-maintenance data.
- The hydraulic release profile uses cycle-level summary statistics from a selected sensor subset to keep runtime bounded.
- HAR, C-MAPSS, ARCENE, ACS/Folktables, Bike Sharing, Appliances Energy, and OpenML breadth suites remain candidates for later validation and are not claimed as executed in 1.4.0.
- External-anchor alerts do not establish causal bias or invalidity; they identify stress-sensitive evidence that requires scientific interpretation.
