# ECG Governance Demo

An atrial fibrillation (AFib) detector built on public ECG data, wrapped in the governance layer a clinical deployment would need: confidence-based routing, an authority matrix, a safety pre-filter for critical findings, and stop mechanisms adapted from clinical lab quality control (Westgard rules).

This is a portfolio demo, not a clinical tool. The governance checklist at the end of the notebook lists what it does and does not implement.

Built by Lindsay Guvenc, part of the [Healthcare Governance Commons](https://healthcare-governance-commons.vercel.app).

## What's in the notebook

`ecg_governance_demo.ipynb`, in order:

1. **Data.** About 800,000 ECG machine-measurement records from MIMIC-IV-ECG (161,327 unique patients).
2. **Detector.** A simple rule-based AFib signal (absent P-wave axis), then a multi-feature confidence score.
3. **Patient-level evaluation.** One reading per patient, to remove the inflation from repeat ECGs on the same person.
4. **Routing.** Six tiers: Confident Normal, Confident AFib, and four human-review urgency levels (Critical, Urgent, Soon, Routine).
5. **Authority matrix.** Who or what acts on each tier, and the escalation path.
6. **Critical safety pre-filter.** Added after finding that STEMI ("consider acute ST elevation MI") cases were being auto-routed as normal or AFib. Critical findings now bypass automatic routing.
7. **Governance monitor.** Rolling sensitivity and false-positive checks against set thresholds, with alerts and failover, run on a time-ordered holdout (first 70% to build, last 30% to monitor).
8. **Dashboard** (`governance_dashboard.png`) and an **honest-documentation checklist** of what is and isn't implemented.

## Known limitations

- Labels come from the ECG machine's own interpretation text, not cardiologist review.
- Single health system data; retrospective only; no subgroup (age, sex, race) analysis.
- Thresholds are illustrative, set from published device benchmarks, not validated for clinical use.

## Running it

1. Download `machine_measurements.csv` from [MIMIC-IV-ECG on PhysioNet](https://physionet.org/content/mimic-iv-ecg/1.0/) (open access) and place it in this folder. The data file is not included in this repository.
2. `pip install -r requirements.txt`
3. Open the notebook in Jupyter and run all cells.

## Data citation

This project uses MIMIC-IV-ECG, released under the Open Data Commons Open Database License v1.0.

Gow, B., Pollard, T., Nathanson, L. A., Johnson, A., Moody, B., Fernandes, C., Greenbaum, N., Waks, J. W., Eslami, P., Carbonati, T., Chaudhari, A., Herbst, E., Moukheiber, D., Berkowitz, S., Mark, R., & Horng, S. (2023). *MIMIC-IV-ECG: Diagnostic Electrocardiogram Matched Subset* (version 1.0). PhysioNet. https://doi.org/10.13026/4nqg-sb35

Goldberger, A., Amaral, L., Glass, L., Hausdorff, J., Ivanov, P. C., Mark, R., ... & Stanley, H. E. (2000). PhysioBank, PhysioToolkit, and PhysioNet: Components of a new research resource for complex physiologic signals. *Circulation*, 101(23), e215–e220.
