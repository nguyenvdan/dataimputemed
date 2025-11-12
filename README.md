# Impute-Pro
MICE-based imputation pipeline for biomedical data with uncertainty quantification. Achieves 2.34% AUC improvement over baseline on diabetes prediction via Rubin's Rules ensemble. Includes MAR/MCAR simulation and per-value confidence scoring.

**What This Does:**
Medical datasets often have missing values - sometimes 20-30% of the data is incomplete because tests weren't done, equipment failed, or records are incomplete. Most basic approaches like filling with the mean don't work well because they ignore relationships between features and don't tell you how confident the imputation is. This project uses MICE to create multiple versions of the complete dataset, then measures how different those versions are to estimate uncertainty. I also implemented "Rubin's Rules" which is a proper way to combine predictions from models trained on each imputed version. I tested it on the Pima Indians Diabetes dataset and got a 2.34% improvement in AUC score compared to basic mean imputation. The system also outputs uncertainty scores for each imputed value so you know which predictions to trust.

**Results**
Dataset: Pima Indians Diabetes (768 patients, 8 features like glucose, blood pressure, BMI)
Missing data: 30% simulated 
Performance:
Simple mean imputation: 0.8226 AUC
MICE with Rubin's Rules: 0.8419 AUC
Improvement: +2.34%

The uncertainty scores ranged from 0.0006 to 0.8875, with an average of 0.1054 for imputed cells. Higher uncertainty means the algorithm was less confident about what the missing value should be.
