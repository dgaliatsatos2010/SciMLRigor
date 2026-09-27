# SciMLRigor 1.2.0 — R02–R10 Forensic Upgrade

SciMLRigor 1.2.0 extends the forensic methodology introduced in R01 across the entire canonical R01–R10 suite while preserving the 1.x API.

Highlights:
- dependence-aware nested selection and tuning diagnostics (R02);
- group structure and worst-group forensics (R03);
- drift, rolling-window, time-order and embargo diagnostics (R04);
- exact and near-duplicate forensics (R05);
- group-preserving and time-block permutation nulls (R06);
- joint/per-field identifier shortcut tests (R07);
- per-feature and feature-group ablations (R08);
- domain shift, worst-domain and pairwise transfer diagnostics (R09);
- binary calibration plus regression reliability stress testing (R10);
- unified report helpers: `forensic_profile()`, `forensic_findings()`, `severe_forensic_findings()`.

Compatibility: existing R01–R10 codes and core fields are retained; all new options are additive.
