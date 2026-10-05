# Churn notebook review

The notebook was directly edited and executed. Original test membership and CSV contents are preserved; IDs are saved and checked on future runs. The split was independently reproduced from the original calls: 146,131 training, 48,711 validation and 48,711 test customers.

## Changes

- Explicitly distinguish verified binary encoding from unverified business semantics; churn=1 is the positive project convention.
- Acknowledge the previously inspected test and reused validation. Remove claims of an untouched test or a fresh independent final evaluation.
- Audit negative usage, duplicate records, date parsing, numeric overlap and geographic consistency without asserting synthetic origin.
- Keep deterministic cleaning separate from fold-fitted imputation, encoding and scaling; remove the undocumented calls-plus-SMS sum.
- Add Extra Trees and weighted/unweighted versions of all four model families.
- Run two RandomizedSearchCV draws per family/weighting mode with three identical training folds: 48 CV fits plus refits. AP chooses the specification on training CV; validation chooses the F1 threshold.
- Record every sampled configuration, CV supporting metrics and fold variability; compare train/CV and train/validation AP gaps.
- Compare F1 with exact 10%/20% outreach, majority-class and contact-everyone baselines, plus finite-sample random-ranking simulations.
- Add paired bootstrap intervals for AP minus prevalence and AUC minus 0.5. These condition on fixed scores and omit search/retraining uncertainty.
- Retain validation permutation importance and a frozen descriptive test audit. No test result feeds a subsequent choice.

## Measured outcome

Unweighted Extra Trees wins CV AP at **0.20330** (fold SD 0.00171) versus baseline **0.20048**. Validation AP is **0.20068**, prevalence **0.20049**, and ROC-AUC **0.49562**. Full-training AP is **0.40586**, so the train/validation gap is **0.20519**. Weighting does not consistently improve ranking.

Validation F1 tuning yields precision **0.20064**, recall **0.99908**, F1 **0.33417**, accuracy **0.20178**, contacting **99.83%**. Contact-everyone F1 is **0.33401**; majority-class accuracy is **0.79951**. Validation 10%/20% lift is **1.01456 / 1.01722**. Paired-bootstrap 95% intervals include zero: AP excess **[-0.00339, 0.00410]**, AUC excess **[-0.01124, 0.00191]**.

The already inspected test audit has AP **0.20305**, AUC **0.50403**, and exact 10%/20% lift **0.99930 / 0.98456**. This is descriptive, not a fresh final claim. There is no dependable outreach improvement to justify deployment or force higher metrics.

## Quality evidence and next data

There are no original missing entries or exact duplicate rows. Negative calls/SMS/data counts are **6,713 / 7,375 / 6,050**. Every city spans all 28 states. Numeric standardized class-mean differences are below **0.011**. These observations establish quality concerns and weak observed associations; they do not establish synthetic origin or label correctness.

Obtain source provenance and definitions, observation timestamps and a future churn window. Add genuinely pre-outcome contract, payment, billing, complaint, outage and usage-trend records with documented measurement windows/units. Compute tenure only with a real observation date. Obtain a new independent temporal dataset and randomized retention-outcome evidence.

## Verification

The notebook completed all 41 code cells sequentially from a fresh kernel with saved outputs and no cell errors. Notebook schema and all Python cell syntax validate. Metric warning policies were subsequently made explicit without changing measured metric values. The original CSV SHA256 is `57d1a2629cad4c63ddd1aaea51da074f7112274d16d810cee4944ed3827767ae`.
