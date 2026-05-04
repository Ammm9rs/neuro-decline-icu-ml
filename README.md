# neuro-decline-icu-ml
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

## Project Structure
neuro-decline-icu-ml/
│
├── sql/
│   ├── label_construction.sql      # ICD + proxy + control cohort
│   └── feature_extraction.sql      # 15 clinical features via BigQuery
│
├── notebooks/
│   └── FBI_final_model.ipynb       # Full pipeline: clean → train → evaluate
│
├── outputs/
│   ├── plot0_leakage_comparison.png
│   ├── plot1_feature_importance.png
│   ├── plot2_pr_curve.png
│   ├── plot3_confusion_matrix.png
│   ├── cohort_visualization.png
│   ├── threshold_selection.png
│   ├── clinical_impact.png
│   ├── shap_global.png
│   └── shap_local.png
│
├── neuro_decline_model.pkl         # Trained XGBoost model
├── requirements.txt
└── README.md
