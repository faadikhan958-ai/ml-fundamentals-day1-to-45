# Restaurant Tip Classifier

**Ticket:** A restaurant chain wants to know which customers are likely to leave an above-average tip, so staff can give them better attention.

## Data
Seaborn's built-in `tips` dataset (244 restaurant bills).

## Approach
- Target: `is_high_tip` = 1 if the tip is above the median tip, otherwise 0
- Features: `total_bill`, `size` (party size)
- Model: Decision Tree (max depth 3)
- Evaluation: 5-fold cross-validation, reporting mean and std

## Observations
- A tip naturally scales with the bill, so this target may mostly learn "big bill means big tip". A better framing would be tip *percentage* of the bill, which removes that effect.
- With only 244 rows, fold-to-fold variation is large, which makes this a good small-data practice case for cross-validation.

## Run it
```bash
pip install -r requirements.txt
jupyter notebook
```
Open the notebook in this folder and run all cells.
