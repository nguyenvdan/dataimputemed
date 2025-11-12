# Impute-Pro: Missing Data Imputation for Medical Datasets

A machine learning project that handles missing data in medical datasets using MICE (Multiple Imputation by Chained Equations). Built as part of learning data science and healthcare AI applications.

## What This Does

Medical datasets often have missing values. Many times 20-30% of the data is incomplete because tests weren't done, equipment failed, or records are incomplete. Most basic approaches like filling with the mean don't work well because they ignore relationships between features and don't tell you how confident the imputation is.

This project uses MICE to create multiple versions of the complete dataset, then measures how different those versions are to estimate uncertainty. "Rubin's Rules" was implemented, which is a proper way to combine predictions from models trained on each imputed version.

I tested it on the Pima Indians Diabetes dataset and got a 2.34% improvement in AUC score compared to basic mean imputation (0.8226). The system also outputs uncertainty scores for each imputed value so you know which predictions to trust.

## Results

Dataset: Pima Indians Diabetes (768 patients, 8 features like glucose, blood pressure, BMI)

Missing data: 30% simulated 

Performance:
- Simple mean imputation: 0.8226 AUC
- MICE with Rubin's Rules: 0.8419 AUC
- Improvement: +2.34%

The uncertainty scores ranged from 0.0006 to 0.8875, with an average of 0.1054 for imputed cells. 

## How It Works

### 1. Creating Missing Data

To test the imputation, I first simulated realistic missing data patterns. I made blood pressure measurements more likely to be missing when glucose is high (50% chance) vs low (1% chance). This mimics clinical scenarios where certain tests are skipped based on other measurements. Then I randomly removed more values to reach 30% total missing data.

### 2. MICE Imputation

MICE works by going through each feature with missing values and predicting them using all the other features. I used ExtraTrees (a type of random forest) as the predictor because it can handle non-linear relationships. The algorithm does this 5 times with different random seeds, creating 5 complete datasets that are all slightly different.

### 3. Measuring Uncertainty

For each missing value, I calculated the standard deviation across the 5 imputations. If all 5 versions agree on approximately the same value, the uncertainty is low. If they disagree a lot, the uncertainty is high.

### 4. Testing with Machine Learning

I trained 5 separate XGBoost models (one on each imputed dataset) and averaged their predictions. This is called Rubin's Rules and it's the statistically correct way to handle multiple imputation. I compared this against a baseline that just uses mean imputation with one model.

## Installation

```bash
git clone https://github.com/YOUR_USERNAME/impute-pro.git
cd impute-pro
pip install -r requirements.txt
```

Python 3.8 required. The requirements.txt includes numpy, pandas, matplotlib, seaborn, scikit-learn, and xgboost.

## Usage

```bash
python impute_pro.py
```

Creates two files:

1. final_imputed_data_with_uncertainty.csv - the imputed data with flags and uncertainty scores
2. impute_pro_results.png - visualizations showing the results

The CSV has 24 columns for the 8 features:
- First 8 columns: the actual imputed values
- Next 8 columns: flags (1 = imputed, 0 = original data)
- Last 8 columns: uncertainty scores

## What I Learned

Simple approaches like mean imputation seem okay but they can hurt model performance and give you no idea when to be cautious about predictions. MICE is more complex but gives better results and the uncertainty estimates are really useful.

The 2.34% improvement might seem small but in medical applications even small gains matter. More importantly, knowing which predictions are uncertain is valuable. In a real clinical setting you'd want to flag those cases for human review or additional testing.

I also learned about proper statistical methods (Rubin's Rules) for combining multiple imputed datasets, which is something I hadn't seen in my coursework before. The implementation was challenging but the scikit-learn IterativeImputer made it manageable.

## Files

```
impute-pro/
├── impute_pro.ipynb
├── README.md
├── requirements.txt
├── LICENSE

```

## Technologies Used

Python, scikit-learn, XGBoost, pandas, numpy, matplotlib

Key concepts: MICE imputation, ensemble methods, uncertainty quantification, Rubin's Rules

## Author

Daniel Nguyen

Feel free to reach out if you have questions or suggestions!

## License

MIT License
