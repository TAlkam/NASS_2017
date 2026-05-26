# Ambulatory Surgery Vulnerability in Alzheimer’s Disease

This repository contains the analytic code for the study:

**Ambulatory Surgery Vulnerability in Alzheimer’s Disease: Non-routine Discharge and Explainable Machine Learning in an Age- and Sex-Matched National Cohort**

## Overview

This study examines whether Alzheimer’s disease is associated with non-routine discharge after major ambulatory surgery using the 2017 Healthcare Cost and Utilization Project Nationwide Ambulatory Surgery Sample (HCUP NASS).

The analysis uses an age- and sex-matched cohort of older adults undergoing ambulatory surgery. Alzheimer’s disease encounters were identified using ICD-10-CM diagnosis codes beginning with `G30`. All Alzheimer’s disease encounters were retained and matched 1:1 to non-Alzheimer’s comparator encounters within identical age and sex strata.

The primary outcome is non-routine discharge after ambulatory surgery. Secondary outcomes include total charges, high-charge encounters, diagnosis burden, procedure burden, and procedure-category patterns. The study combines regression analysis with explainable machine learning to characterize risk patterns associated with non-routine discharge.

## Study Design

This is a retrospective encounter-level analysis of the 2017 HCUP Nationwide Ambulatory Surgery Sample.

The final matched analytic cohort includes:

- Alzheimer’s disease encounters: 6,968
- Age- and sex-matched non-Alzheimer’s encounters: 6,968
- Total matched cohort: 13,936 encounters

Because the analytic cohort was intentionally matched at a 1:1 ratio, the matched dataset is used for comparative outcome analysis and not for estimating the national prevalence of Alzheimer’s disease among ambulatory surgery encounters.

## Main Research Question

Among older adults undergoing major ambulatory surgery, is Alzheimer’s disease associated with a higher likelihood of non-routine discharge compared with age- and sex-matched non-Alzheimer’s encounters?

## Key Variables

### Exposure

Alzheimer’s disease was defined using ICD-10-CM diagnosis codes beginning with:

```text
G30
```

The binary exposure variable is:

```text
alz = 1 for Alzheimer’s disease encounters
alz = 0 for non-Alzheimer’s comparator encounters
```

### Primary Outcome

Non-routine discharge was defined from the disposition variable:

```text
nonroutine = 0 for routine discharge
nonroutine = 1 for non-routine discharge
```

Non-routine discharge includes transfer, home health care, discharge against medical advice, death, or unknown/non-routine destination depending on disposition coding.

### Secondary Outcomes

Secondary outcomes include:

```text
totchg          Total charges
log_totchg      Log-transformed total charges
high_charge     Top-decile charge encounter
i10_ndx         Number of ICD-10-CM diagnoses
ncpt_inscope    Number of in-scope CPT procedures
cptccs1         Principal CPT/CCS procedure category
```

## Analysis Workflow

The code performs the following steps:

1. Load the HCUP NASS 2017 Stata dataset.
2. Identify Alzheimer’s disease encounters using ICD-10-CM `G30.x` codes.
3. Create an age- and sex-matched Alzheimer’s/non-Alzheimer’s cohort.
4. Define non-routine discharge and charge-related outcomes.
5. Generate exploratory descriptive tables and figures.
6. Run logistic regression models for non-routine discharge.
7. Run regression models for log total charges and high-charge encounters.
8. Train machine learning models to predict non-routine discharge.
9. Evaluate model performance using AUROC, AUPRC, accuracy, precision, recall, and F1 score.
10. Apply SHAP to interpret feature contributions in the XGBoost model.
11. Export manuscript-ready tables and figures.

## Main Statistical Models

The primary regression analysis estimates the association between Alzheimer’s disease and non-routine discharge using sequential logistic regression models.

The models are structured as follows:

```text
Model 1: Alzheimer’s disease only
Model 2: Alzheimer’s disease + age + sex
Model 3: Model 2 + payer, ZIP income quartile, patient location
Model 4: Model 3 + diagnosis count and procedure count
Model 5: Model 4 + weekend surgery and calendar quarter
```

Results are reported as odds ratios with 95% confidence intervals.

## Machine Learning Models

The prediction target is non-routine discharge.

The following models are evaluated:

```text
Logistic regression
Random forest
XGBoost
```

Performance metrics include:

```text
AUROC
AUPRC
Accuracy
Precision
Recall
F1 score
```

SHAP analysis is used to interpret XGBoost predictions and identify features contributing to predicted non-routine discharge risk. SHAP values are interpreted as model-based prediction contributions and not as causal effects.

## Main Outputs

The code generates manuscript-ready tables including:

```text
Table 1. Baseline characteristics by Alzheimer’s disease status
Table 2. Discharge disposition, charge burden, and procedure burden
Table 3. Logistic regression models for non-routine discharge
Table 4. Regression models for charge-related outcomes
Table 5. Machine learning performance for non-routine discharge prediction
```

The code also generates figures including:

```text
Figure 1. Discharge disposition by Alzheimer’s disease status
Figure 2. Diagnosis burden by Alzheimer’s disease status
Figure 3. Receiver operating characteristic curve for non-routine discharge prediction
Figure 4A. Receiver operating characteristic curve for prediction model performance
Figure 4B. SHAP summary plot for explainable machine learning predictors
```

Depending on journal formatting, ROC and SHAP outputs may be combined into a single multi-panel Figure 4.

## Repository Structure

A suggested repository structure is:

```text
NASS_2017_AD_Ambulatory_Surgery/
│
├── README.md
├── code/
│   ├── 01_data_preparation_matching.ipynb
│   ├── 02_exploratory_analysis.ipynb
│   ├── 03_regression_models.ipynb
│   ├── 04_machine_learning_shap.ipynb
│   └── 05_tables_figures_export.ipynb
│
├── outputs/
│   ├── tables/
│   └── figures/
│
└── docs/
    └── variable_notes.md
```

The HCUP NASS dataset itself should not be uploaded to GitHub.

## Data Availability

The data used in this study were obtained from the Healthcare Cost and Utilization Project Nationwide Ambulatory Surgery Sample, 2017. HCUP data are restricted-access data and cannot be publicly redistributed by the authors.

Researchers may obtain access to HCUP NASS directly from the Agency for Healthcare Research and Quality after completing the required HCUP Data Use Agreement training and obtaining the relevant dataset through HCUP.

This repository does not contain raw HCUP data.

## Code Availability

The analytic code for cohort construction, matching, outcome definition, regression modeling, machine learning, SHAP explainability, and figure generation is provided in this repository.

To reproduce the analyses, users must have authorized access to the 2017 HCUP NASS dataset and must update the file paths in the notebooks to point to their local or Google Drive copy of the dataset.

## Software Requirements

The analysis was conducted in Google Colab using Python.

Main Python packages include:

```text
pandas
numpy
pyreadstat
matplotlib
statsmodels
scikit-learn
xgboost
shap
openpyxl
```

Install required packages in Colab using:

```python
!pip install pyreadstat xgboost shap openpyxl -q
```

## Reproducibility Notes

The matching procedure uses a fixed random seed:

```python
random_state = 20260524
```

This ensures that the age- and sex-matched comparator cohort can be reproduced when using the same input dataset and code.

The machine learning train-test split also uses the same random seed for reproducibility.

## Important Interpretation Notes

This study is based on administrative encounter-level data. Regression results should be interpreted as associations rather than causal effects.

The matched cohort is designed for comparative analysis between Alzheimer’s disease and non-Alzheimer’s encounters. It should not be used to estimate the population prevalence of Alzheimer’s disease among all NASS ambulatory surgery encounters.

Machine learning analyses are exploratory and intended to characterize multivariable risk patterns. The models are not intended for direct clinical deployment without external validation and prospective evaluation.

## Citation

If using this code or adapting this workflow, please cite the corresponding manuscript:

```text
Alkam T, et al. Ambulatory Surgery Vulnerability in Alzheimer’s Disease: 
Non-routine Discharge and Explainable Machine Learning in an Age- and 
Sex-Matched National Cohort.
```

## Contact

For questions about the code or analysis workflow, please contact:

```text
Tursun Alkam, MD, PhD
Email: tursun.alkam@gmail.com
```
