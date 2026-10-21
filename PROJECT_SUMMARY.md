# Project Summary

## Aim
Explore prediction of recorded <30-day hospital readmission in the UCI diabetes encounters dataset, including calibration and subgroup performance.

## Research contribution
The project emphasizes patient-level splitting to reduce leakage across partitions, calibration of probability estimates, and transparent subgroup reporting with uncertainty.

## Responsible interpretation
This is exploratory work using historical 1999–2008 data. The dataset does not establish feature availability time, so the model is not validated for admission-time use. Results should not be used for patient care.

## Portfolio description (draft)
Developed an exploratory machine-learning workflow using a historical U.S. hospital diabetes dataset to study recorded 30-day readmission. Implemented patient-level train/validation/test splitting, logistic regression, probability calibration, performance evaluation, and subgroup comparisons across race, gender, and age. Documented data limitations, uncertainty, and the need for external clinical validation.
