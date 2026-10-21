# Predicting 30-Day Hospital Readmission in Patients with Diabetes

An exploratory machine-learning project using the **UCI Diabetes 130-US Hospitals for Years 1999–2008** dataset. The project evaluates a logistic-regression baseline, probability calibration, and subgroup performance for a recorded `<30` readmission outcome.

> **Important:** This is an educational/research project, not a clinical decision tool. It has not been clinically validated and must not be used for patient-care decisions.

## Research question
How well can a baseline model distinguish encounters with a recorded readmission within 30 days, how reliable are its predicted probabilities, and do performance metrics vary across recorded race, gender, and age groups?

## Dataset
Use the UCI dataset titled *Diabetes 130-US Hospitals for Years 1999–2008*. The ZIP should contain `diabetic_data.csv` and `IDS_mapping.csv`. The dataset is not included in this repository; download it from the official UCI Machine Learning Repository and follow its data-use terms.

For local execution, place the ZIP file beside the notebook. In Google Colab, upload the ZIP or update the `DATASET_ZIP` path in the first code cell.

## Outcome definition
The positive class is `readmitted == '<30'`; the negative class combines `'>30'` and `'NO'`. This means no readmission was recorded in this dataset under the chosen label, not that a person was never readmitted elsewhere or later.

## Methodology
- Replace `?` values with missing values during preprocessing.
- Exclude encounter and patient identifiers from predictors; use patient IDs only to create non-overlapping train/validation/test groups.
- Exclude `weight` because of extreme missingness and `discharge_disposition_id` because it may be outcome-adjacent.
- Fit a logistic-regression baseline with numeric imputation/scaling and categorical imputation/one-hot encoding.
- Fit sigmoid and isotonic calibration using the validation partition and evaluate on the held-out test partition.
- Report ROC-AUC, average precision, Brier score, log loss, calibration error, and subgroup recall/false-positive rates at an explicitly illustrative threshold.
- Estimate selected metric intervals using patient-level bootstrap resampling.

## Example results from the executed notebook

The notebook was executed with the repository's fixed random seeds and the dataset ZIP. In this run, the patient-level partitions contained 71,195 training encounters, 15,264 validation encounters, and 15,307 test encounters; no patient appeared in more than one partition. The test positive rate was about 10.7%.

| Model | ROC-AUC | Average precision | Brier score | Log loss |
|---|---:|---:|---:|---:|
| Uncalibrated logistic regression | 0.6368 | 0.1851 | 0.0933 | 0.3292 |
| Sigmoid calibrated | 0.6368 | 0.1851 | 0.0929 | 0.3280 |
| Isotonic calibrated | 0.6342 | 0.1740 | 0.0930 | 0.3327 |

Sigmoid calibration modestly improved the Brier score and log loss in this run, while discrimination remained limited (ROC-AUC around 0.64). Isotonic calibration had a slightly lower 10-bin expected calibration error but lower average precision. These differences are exploratory and do not establish clinical utility.

At the illustrative 0.12 threshold, subgroup recall and false-positive rates varied. For example, test-set recall was approximately 47.5% for female encounters and 51.4% for male encounters; false-positive rates were approximately 30.6% and 30.3%, respectively. Smaller race and age groups have limited positive-case counts, so their metrics are especially uncertain. See `outputs/subgroup_performance.csv` and `outputs/bootstrap_confidence_intervals.csv` for full tables.

Calibration plot: [`outputs/calibration_plot.png`](outputs/calibration_plot.png). These numbers correspond to this notebook's current experiment and should not be mixed with results from earlier runs using different splits or settings.

## Prediction-time caveat
The feature set contains encounter-level information including length of stay, lab/procedure counts, medication information, and diagnoses. The dataset does not establish exactly when all variables become available. Therefore, this should **not** be described as a validated admission-time prediction system. `discharge_disposition_id` is excluded as a potentially outcome-adjacent variable, but other feature timing must still be reviewed before any intended-use claim.

## Run the notebook
1. Install dependencies: `pip install -r requirements.txt`.
2. Download the dataset ZIP and place it next to `diabetes_readmission_analysis.ipynb`.
3. Open the notebook in Jupyter or Google Colab.
4. Run all cells from top to bottom.
5. Review generated CSV files in the `outputs/` folder.

## Repository contents
- `diabetes_readmission_analysis.ipynb` — complete analysis workflow.
- `requirements.txt` — Python dependencies.
- `outputs/` — example outputs generated when the notebook is executed (results may vary if code or split settings change).

## Limitations
- The dataset is historical (1999–2008) and may not represent contemporary clinical practice.
- The outcome is a recorded label and does not prove absence of readmission outside the recorded dataset.
- The feature availability times are not established, so prediction timing is unresolved.
- Subgroup metrics are descriptive, threshold-dependent, and uncertain for smaller groups. A single metric cannot establish whether a model is fair.
- No external, prospective, or clinical validation has been performed.
- Predictive associations do not establish causal relationships.

## Citation
Be sure to cite the UCI Machine Learning Repository dataset record and the original dataset publication in your final academic submission. Verify the required citation from the official dataset page before submitting.
