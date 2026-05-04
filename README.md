# Neuro-decline-icu-ml
Early detection of neurological decline in ICU patients  using MIMIC-IV clinical data and XGBoost — FBI Project, IIIT Delhi 2026

# Neurological Decline Detection in ICU Patients

Early warning system for identifying ICU patients experiencing 
catastrophic neurological decline — enabling timely organ preservation 
and transplant coordination.

Built using real clinical data from MIMIC-IV v3.1 via Google BigQuery.

---

## Motivation

Inspired by Dr. Samrath (ICU Physician, Dubai Hospital) who highlighted 
that delayed recognition of brain death costs transplant windows. 
This project is a direct response to that clinical problem.

---

## Results

| Metric | Value |
|--------|-------|
| AUC-ROC | 0.9098 |
| Recall (threshold 0.3) | 87.3% |
| Precision (threshold 0.3) | 35.1% |
| Cases caught | 199 / 228 in test set |
| Cases missed | 29 / 228 in test set |

---

## Dataset

- **Source:** MIMIC-IV v3.1 (PhysioNet)
- **Access:** Google BigQuery (physionet-data)
- **Cohort:** 11,350 ICU stays
- **Positives:** 1,142 (ICD G93.82 + neurological proxy)
- **Negatives:** 10,208 (died, non-neurological cause)

> Note: MIMIC-IV requires credentialed PhysioNet access.
> Raw data is NOT included in this repository.

---

---

## Key Findings

**1. Label Construction from Scratch**
MIMIC-IV has no brain death column. We combined:
- ICD-10 code G93.82 → 129 gold standard cases
- Clinical proxy (GCS ≤ 8 + InvasiveVent + Died) → 1,013 cases

**2. Data Leakage Detected and Fixed**
`on_invasive_vent` was used to build the label AND as a feature — 
classic circular logic. Removing it dropped AUC by only 0.018, 
confirming genuine learned signal.

**3. Class Imbalance as a Design Problem**
Predicting perfect organ donors (362 cases) gave recall near zero. 
Reframing to neurological decline only (1,142 cases) fixed the imbalance 
at the problem definition level, not the math level.

**4. SHAP Explainability**
Global and local SHAP values confirm the model learned clinically 
correct patterns — low GCS drives predictions, stable MAP reduces them.

---

## Model

- **Algorithm:** XGBoost
- **scale_pos_weight:** 8.94 (handles 1:9 class imbalance)
- **Threshold:** 0.3 (recall prioritized for clinical alert system)
- **Features:** 15 clinical features across neurological, 
  hemodynamic, organ, and composite categories

---

## Limitations

- Proxy labels carry uncertainty — algorithmic ≠ ground truth
- Single center data (BIDMC only)
- Static features — no time-series trajectory modeling
- Missing indicator not implemented
- No external validation dataset

---

## Future Work

- LSTM/Transformer for time-series trajectory modeling
- Missing indicator features for lab values
- External validation on eICU or AmsterdamUMCdb
- Prospective clinical evaluation with transplant coordinators

---

## Requirements
pip install -r requirements.txt

xgboost
scikit-learn
pandas
numpy
matplotlib
seaborn
shap
joblib

---

## Authors

**Ammar Jamil**
B.Tech CSB | IIIT Delhi | 2024065
ammar24065@iiitd.ac.in

---

## Acknowledgements

- Dr. Samrath (ICU Physician, Dubai Hospital) — clinical motivation
- Prof. Tavpritesh Sethi — FBI course guidance
- PhysioNet / MIMIC-IV team — data access
- IIIT Delhi

---

## Citation

If you use this work:
