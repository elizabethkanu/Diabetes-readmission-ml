# Results Summary

These results come from the executed `diabetes_readmission_analysis.ipynb` using its fixed random seeds and a patient-level train/validation/test split. Do not combine these values with results from other experiments using different splits or settings.

## Split

| Partition | Encounters | Unique patients | Positive rate |
|---|---:|---:|---:|
| Train | 71,195 | 50,062 | 11.24% |
| Validation | 15,264 | 10,728 | 11.25% |
| Test | 15,307 | 10,728 | 10.70% |

No patient overlap was present across partitions.

## Test-set model metrics

| Model | ROC-AUC | Average precision | Brier score | Log loss | ECE (10 bins) |
|---|---:|---:|---:|---:|---:|
| Uncalibrated logistic regression | 0.6368 | 0.1851 | 0.0933 | 0.3292 | 0.0096 |
| Sigmoid calibrated | 0.6368 | 0.1851 | 0.0929 | 0.3280 | 0.0051 |
| Isotonic calibrated | 0.6342 | 0.1740 | 0.0930 | 0.3327 | 0.0048 |

Sigmoid calibration modestly improved Brier score and log loss; ranking performance remained limited. Isotonic had the lowest ECE but somewhat lower average precision. ECE depends on binning and should not be interpreted alone.

## Example subgroup results at illustrative threshold 0.12

- Female: recall 47.5%; false-positive rate 30.6% (8,283 encounters; 899 positive cases).
- Male: recall 51.4%; false-positive rate 30.3% (7,024 encounters; 739 positive cases).
- Recorded race groups vary in size; some categories have fewer than 100 encounters and very few positive cases. Interpret subgroup estimates cautiously.
- Age-group results also vary; small groups have unstable estimates.

## Interpretation and limitations

The model's ROC-AUC around 0.64 indicates limited discrimination. Calibration improved some probability metrics but does not improve ranking. Subgroup differences are descriptive and do not alone prove fairness or unfairness. The dataset covers 1999–2008, feature availability times are not established, and no external or clinical validation has been performed. The work is not suitable for patient-care decisions.
