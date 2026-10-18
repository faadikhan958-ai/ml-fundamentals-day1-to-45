# Loan Approval Prediction

Comparing classical ML models (Decision Tree, KNN, SVM) on a real, messy loan-application dataset, with baselines, cross-validation, and honest reporting of what the numbers do and do not show.

## Problem
Predict whether a loan application will be approved, using applicant financial and demographic data.

## Data
- Kaggle loan-approval dataset (`train.csv`): 614 applicants, 13 columns, target `Loan_Status`
- Messy as delivered: missing values in numeric and categorical columns, a text value (`3+`) inside `Dependents`, categories stored as text

## Cleaning
- Numeric columns (`LoanAmount`, `Loan_Amount_Term`, `Credit_History`): filled with the median
- Categorical columns (`Gender`, `Married`, `Dependents`, `Self_Employed`): filled with the mode
- `Dependents`: `3+` replaced with 3, then converted to a number
- Binary categories encoded with `map()`; `Property_Area` (3 categories) encoded the same way

## Method
1. Baseline: always predict the majority class
2. 80/20 train/test split; `StandardScaler` fitted on the training part only, used for KNN and SVM
3. Models: Decision Tree (depth 4), KNN (k=5), SVM (linear and RBF)
4. 5-fold cross-validation on the training part to check stability, not just one lucky split

## Results

Baseline (majority class): 68.7% on the full data, 65.0% on the held-out test split.

**Single test split**

| Model | Accuracy |
|---|---|
| Baseline | 65.0% |
| Decision Tree (depth 4) | 75.6% |
| KNN, unscaled | 58.5% |
| KNN, scaled | 78.9% |
| SVM, linear | 78.9% |
| SVM, RBF | 79.7% |

**5-fold cross-validation**

| Model | Mean | Std |
|---|---|---|
| Decision Tree | 79.2% | 0.011 |
| KNN | 79.0% | 0.023 |
| SVM, linear | 81.5% | 0.021 |
| SVM, RBF | 80.9% | 0.014 |

## What I learned
- **Scaling matters for distance-based models:** unscaled KNN scored *below* the baseline (58.5%); scaling lifted it to 78.9%.
- **One split can mislead:** SVM-RBF led on the single split, but SVM-linear led under cross-validation.
- **Accuracy is not the only criterion:** the Decision Tree was the most stable (lowest std) and the easiest to explain, while SVM-linear had the best mean. The right pick depends on whether explainability matters.
- **Credit history dominated:** in the Decision Tree, `Credit_History` carried about 73% of the feature importance; gender and education had almost none.

## Limitations
- Small dataset (614 rows): differences of about one point between models are within noise.
- Cross-validation ran on the training part only; there is no external validation set.
- A model like this should support, not replace, a human lending decision.

## Run it
```bash
pip install -r requirements.txt
jupyter notebook
```
Open the notebook in this folder and run all cells.
