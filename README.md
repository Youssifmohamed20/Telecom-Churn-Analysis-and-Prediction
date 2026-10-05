# Telecom Customer Churn Analysis and Prediction

A reproducible audit of whether the recorded customer features support churn ranking and retention outreach.

**Measured finding:** training-CV-selected unweighted Extra Trees has validation AP **0.20068**, versus prevalence **0.20049**, and ROC-AUC **0.49562**. Its small training CV gain does not replicate on validation. F1 tuning contacts **99.83%** of customers; validation top-10% / top-20% lift is **1.015x / 1.017x**. Uncertainty includes random performance. There is no dependable targeting improvement.

## Files and execution

- `Telecom_Churn_Analysis_and_Prediction.ipynb`: analysis with saved measured outputs and figures.
- `telecom_churn.csv`: existing dataset, unchanged.
- `split_customer_ids.json`: original train/validation/test customer IDs; future runs verify membership.
- `REVIEW_NOTES.md`: methodology, measured results and limitations.
- `requirements.txt`: core runtime dependencies.

Use Python 3.12, install `requirements.txt`, open Jupyter and run the notebook from a fresh kernel with the CSV beside it. All 146,131 training rows are used. The modest search runs 48 CV fits plus refits and takes several minutes; permutation interpretation uses a documented 5,000-row validation subset.

## Evaluation protocol

Preserve the original seed-42 stratified 60/20/20 split. Compare Logistic Regression, Random Forest, Extra Trees and HistGradientBoosting, each weighted and unweighted. Run two randomized configurations per weighting mode with three identical training folds. Keep feature engineering and learned preprocessing inside pipelines. Select by training CV AP; use validation for the F1 threshold and compare exact 10%/20% capacities. Report AP, AUC, churn precision/recall/F1, accuracy, generalization gaps, random-ranking simulations and paired bootstrap intervals.

The test set **was already inspected**. The frozen descriptive audit (AP 0.20305, AUC 0.50403) is not a fresh final claim and does not choose models or thresholds. Reused validation and searched CV also carry selection uncertainty. A new independent temporal cohort is required for a fresh performance claim.

## Data limitations

This project treats churn=1 as positive, but source documentation does not define the business event or horizon. Negative usage, geographic inconsistencies, and overlapping class distributions warrant source validation; synthetic provenance is not established. Registration is a calendar feature, not tenure. No incompatible usage quantities are summed. Collect documented pre-outcome observation windows, contracts, billing/payment changes, complaints, outages and longitudinal usage before further extensive tuning.
