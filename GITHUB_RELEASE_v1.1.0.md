# SciMLRigor 1.1.0 — Leakage Forensics

SciMLRigor 1.1.0 is a backward-compatible feature release focused on outcome leakage that can remain hidden inside engineered or precomputed predictors.

## Highlights

- R01 now combines preprocessing isolation with feature-level leakage forensics.
- New public `leakage_scan(...)` API can be run before model fitting.
- Detects direct/affine target copies, deterministic label transforms, future-target leads, and common rolling/current-target formulas.
- Adds explicit feature-lineage metadata and warns when engineered feature provenance is unknown.
- Adds prediction-horizon / feature-availability checks for look-ahead leakage.
- Formula matching is invariant to affine rescaling, allowing detection after min-max normalization or z-scoring.
- Existing R01 EOG/RS outputs are preserved when a preprocessor is supplied.

## Why this release matters

A correct chronological split or cross-validation scheme does not guarantee leakage-free evaluation. A precomputed predictor can itself contain the current target. SciMLRigor 1.1 therefore treats engineered-feature provenance as part of pipeline isolation rather than assuming that a supplied feature matrix is safe.

## Validation

The release adds seven leakage-specific tests. The complete test suite passes with 27 tests.
