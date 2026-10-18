# Repeat Customer Behaviour

**Business question:** "Which customers will order again?" Given only raw transaction history, with no label provided.

## Data
UCI Online Retail dataset: transaction-level e-commerce data (`CustomerID`, `InvoiceNo`, `InvoiceDate`, `Quantity`, `UnitPrice`).
Source: https://archive.ics.uci.edu/dataset/352/online-retail

Before choosing it, I rejected a customer *survey* dataset: it described general shopping habits but had no record of repeat orders, so no target could be built from it.

## Building the target
- Dropped transactions with no `CustomerID` (a customer cannot be tracked without an ID)
- `repeat_customer = 1` if the customer ordered on **more than one distinct date**
- Counting distinct *dates* instead of rows matters: two items bought on the same day are one visit, not a repeat

Result: 4,372 customers, baseline accuracy **68%** (majority class).

## Features (per customer)
`totalspent`, `avg_order_value`, `total_items`

## The leakage bug (the main lesson)
My first model scored **98.4%** accuracy. That was too good to be true: I had included `num_transactions` as a feature, which is almost the same information the target was built from. The model was reading the answer key.

After removing it: **81.5%** 5-fold CV accuracy against the 68% baseline, with 148 of about 875 test customers misclassified (Decision Tree, depth 4). A smaller, honest number I can trust.

**Rule I now follow:** a score far above the baseline is a reason to audit the feature list before celebrating.

## Limitations
- Features use each customer's *entire* history, including the period after the first repeat order, so the model is not predicting the future in a strict sense. A stricter design would compute features from an early window only and predict orders in a later window.
- Only three behavioural features and one model type were tried.

## Run it
```bash
pip install -r requirements.txt
jupyter notebook
```
Download the dataset from the link above and place the Excel file in this folder, then run the notebook.
