# ML Intrusion Detector

A didactic, end-to-end **network intrusion detection** pipeline — the kind of system a SOC (Security Operations Center) would use to flag suspicious traffic. The notebook walks through the full ML reasoning, not just the code: *why* each model is chosen, *what* the trade-offs mean for a security team, *which* defaults silently hurt performance, and *how* to validate the methodology generalizes.

## What you'll learn

1. **Two complementary detection approaches**  
   - *Isolation Forest* (unsupervised) — catches anything weird, even unknown attacks  
   - *XGBoost* (supervised) — learns specific attack patterns from labeled data
2. **Why default model settings often lie about performance**  
   How a model with 0.97 ROC-AUC can still miss 32% of attacks — and how to fix it (one line of code, +13 F1 points).
3. **Precision vs Recall** — the tension every security team knows, quantified.
4. **Cross-dataset validation** on UNSW-NB15 to prove the methodology isn't a one-trick pony.

## Installation

```bash
pip install -r requirements.txt
```

## Usage

```bash
jupyter notebook threat_detection.ipynb
```

Both datasets are downloaded automatically from public GitHub mirrors at notebook runtime.

## Notebook contents

1. Loading the NSL-KDD dataset
2. Quick exploration
3. Preprocessing (categorical encoding, standardization)
4. Model 1 — Isolation Forest (unsupervised) + intuition
5. Model 2 — XGBoost (supervised) + intuition
6. Comparison & visualizations (confusion matrices, ROC, feature importance, score distribution)
7. Threshold tuning on the Precision-Recall curve — *the silent failure fix*
8. Stacking — combining IF and XGBoost (and an honest take on why it doesn't help here)
9. Comparison of the 4 binary approaches
10. Multi-class classification (Normal / DoS / Probe / R2L / U2R)
11. SMOTE on rare attack classes
12. Cross-dataset validation — UNSW-NB15
13. Conclusion

## Headline results (binary, NSL-KDD)

| Model | Accuracy | F1 | ROC-AUC |
|---|---|---|---|
| Isolation Forest | 0.83 | 0.87 | 0.94 |
| XGBoost (default) | 0.81 | 0.80 | 0.97 |
| **XGBoost (tuned)** | **0.92** | **0.93** | **0.97** |
| Stacking IF + XGB | 0.81 | 0.80 | 0.95 |

Threshold tuning on the PR-curve lifts F1 from 0.80 → 0.93. The same methodology is then re-applied to UNSW-NB15 and yields F1 0.94 with an *opposite* optimal threshold (0.90 vs 0.0001) — strong evidence that the approach generalizes by adapting to whatever bias the data presents.

## Stack

Python · pandas · NumPy · scikit-learn · XGBoost · imbalanced-learn · Matplotlib · Seaborn

## Datasets

- **[NSL-KDD](https://www.unb.ca/cic/datasets/nsl.html)** — improved version of KDD Cup 99 (duplicates removed, balanced distribution). 41 network features, 5 attack categories (Normal / DoS / Probe / R2L / U2R). 148k flows.
- **[UNSW-NB15](https://research.unsw.edu.au/projects/unsw-nb15-dataset)** — modern intrusion detection dataset (UNSW Canberra, 2015). 45 network features, 10 attack categories (Normal, Generic, Exploits, Fuzzers, DoS, Reconnaissance, Analysis, Backdoor, Shellcode, Worms). 257k flows.
