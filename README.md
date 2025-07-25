# scale_presence
Random Forest Model trained to check presence or absence of scales in crops
# Feature Exclusion Impact Analysis on RCS Inference Data (2024)

This repository performs a robust analysis of feature importance using a **Random Forest Classifier** on the 2024 `RCS_Inferred` data. The goal is to evaluate how the exclusion of specific features — particularly environmental, geospatial, and survey-derived — affects model performance and feature ranking.

## Dataset

- **Input file**: `RCS_data_06_23_2025.csv`
- **Target column**: `RCS_Inferred_2024`
- Values in this column are binarized:  
  - `Presence = 1` if `RCS_Inferred_2024 > 0`, else `Presence = 0`

### Feature Groups

- `survey_columns`: All columns starting with `RCS_Inferred_`  
- `base_excluded_cols`: Key columns excluded from all iterations (e.g., surveys, water indicators)  
- `dynamic_excluded_candidates`: Features added incrementally to the exclusion list in each iteration (e.g., `MaxEnt_`, `NRI_`, geospatial variables, and breeding indicators)

---

## Methodology

- For each iteration, one additional feature (from the dynamic exclusion list) is added to the list of excluded features.
- Random Forest Classifier is trained on the remaining features.
- Class balancing is handled by **undersampling** the majority class (`Presence=0`).
- Model performance is evaluated over **10 iterations** (with different random seeds).

### For each iteration, the following metrics are computed:

- Average ROC AUC, Precision, Recall, and F1 Score across all 10 runs.
- Top 15 most important features and their importance scores.

---

## Output

- Final results are saved in:  
  `2024_excluded_feature_impact_summary.csv`

### Columns in the output file:

- `Excluded_Features`: List of excluded features up to that iteration
- `Num_Features`: Number of features used for training
- `AUC_Mean`, `AUC_Std`: Average and std of ROC AUC
- `Precision_Mean`, `Recall_Mean`, `F1_Mean`: Other performance metrics
- `Top_Feature_1` to `Top_Feature_15`: Most influential features for prediction
- `Importance_1` to `Importance_15`: Corresponding feature importances

---

## ▶️ How to Run

### Requirements

Python version: 3.13.5 (tags/v3.13.5:6cb20a2, Jun 11 2025, 16:15:46) [MSC v.1943 64 bit (AMD64)]
pandas version: 2.3.1
numpy version: 2.3.1
scikit-learn version: 1.7.1
matplotlib version: 3.10.3
