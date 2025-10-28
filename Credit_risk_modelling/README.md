# Credit Risk Modelling

## Project Overview

This project demonstrates an end-to-end credit risk modelling workflow for predicting the probability of loan default. The pipeline incorporates data cleaning, exploratory data analysis (EDA), advanced treatment for missing values and class imbalance, feature encoding, as well as supervised machine learning with multiple models. The final output supports risk management and customer credit scoring.

---

## 1. Background

Banks and lenders need to efficiently gauge customer risk profiles and predict defaults to optimize lending decisions and satisfy regulatory demands. As customer portfolios grow and change, hardcoded rules no longer suffice—hence, machine learning is used to build behavioral scorecards using both transactional and demographic data.

The main objective here is to predict the probability of default and use the predictions to develop meaningful credit risk scores and customer rankings.

---

## 2. Data and Preparation

**Data source:** A structured loan dataset with features such as payment history, loan and EMI amounts, tenure, customer demographics, and more. The original data includes some missing values and requires column renaming for clarity.

- Rows: 119,528 (after initial cleaning)
- Target variable: 1 (defaulter), 0 (non-defaulter)
- Imbalance: Only about 2% are defaulters

### Basic Data Cleaning

- Columns are renamed to descriptive labels.
- Missing values are significant in several columns, especially for features like loan amount and customer demographics.
- Some feature columns with excessive missing data are dropped.
- For key columns with occasional missing values, rows are dropped only if values are missing for critical features such as 'Loan Amount' or 'Gender'.

---

## 3. Exploratory Data Analysis (EDA)

- **Correlation matrix:** Numerical features are largely not strongly correlated, suggesting most features provide unique value in modelling.
- **Target analysis:** Severe class imbalance with far fewer defaulters than non-defaulters.
- **Loan amount:** Three types of plots used:
    - **Distribution Plot:** Shows the spread and skew of loan amounts, which can reveal if typical loan amounts differ for defaulters.
    - **Violin Plot (by Tier):** Visualizes loan distribution across different bank customer tiers, indicating risk appetite or historical lending patterns.
    - **Box Plot (by Employment Type):** Highlights central tendency and outliers; for instance, self-employed borrowers may have a wider range or higher median loan sizes.

- **Customer Age:** Histogram reveals most loans are issued at certain age bands, possibly reflecting eligibility and risk policies.
- **Age vs Loan Amount:** Scatterplot exposes any trends in risk by age and loan size, supporting segmentation strategies.

---

## 4. Data Engineering

### 4.1 Handling Missing Data

Different strategies are considered:

- **Row deletion:** Only applied where absolutely necessary to avoid data loss.
- **Imputation:** Median for numerical values, most frequent for categorical. MICE and KNN discussed but not used in final implementation due to complexity and risk of data leakage.

### 4.2 Encoding Categorical Features

- **Label encoding:** Used for categorical features with a natural order or when tree-based models are in use.
- **One-hot encoding:** Used for features without order, preventing models from assuming ordinal relationships.

### 4.3 Addressing Class Imbalance

- **SMOTE (Synthetic Minority Oversampling Technique):** Balances the training dataset by generating synthetic examples for the minority class (defaulters). This prevents the model from ignoring defaulters and improves the ability to generalize.

---

## 5. Modeling

### Workflow

1. **Train-Test Split:** Performed after oversampling to avoid information leakage and ensure fair evaluation.
2. **Algorithms evaluated:**
    - Logistic Regression
    - Random Forest
    - XGBoost
    - Support Vector Machine (SVM)
3. **Model selection criterion:** Both area under the ROC curve (AUC) and F1-score are used, reflecting the need to balance sensitivity and specificity in risk applications.

### Results

| Model                | AUC     | F1 Score | Notes                                                      |
|----------------------|---------|----------|------------------------------------------------------------|
| Logistic Regression  | 0.8483  | 0.7645   | Good recall, moderate precision; simple and interpretable. |
| Random Forest        | 0.9978  | 0.9888   | Near-perfect; top real-world performance.                  |
| XGBoost              | 0.9968  | 0.9876   | Comparable to Random Forest, sometimes more robust.        |
| SVM                  | -       | -        | Results not detailed; typically slower for large data.     |

Random Forest and XGBoost massively outperform Logistic Regression in both discrimination (AUC) and F1-score, making them suitable for real-world risk ranking.

---

## 6. Insights and Model Interpretation

- **Imbalanced classification:** Always evaluate with metrics such as AUC, F1-score, and confusion matrices, as accuracy will be misleading.
- **SMOTE effectiveness:** After balancing, the model not only improves recall (finding more actual defaulters) but usually maintains good precision.
- **Feature importance:** Tree-based methods provide insights into which features most strongly affect the risk prediction (e.g., prior payment behavior, age, and loan amount).
- **Credit scoring:** Probabilities from the best model can be mapped to risk bands for actionable credit-rating (e.g., 0-200: Bad Customer; 201-350: Very High Risk; ...; 700+: Eligible for large loans).

---

## 7. Recommendations and Next Steps

- Refine imputation strategies and compare multiple encoders for categorical variables to optimize predictive power.
- Apply feature engineering for time-based or behavioral aggregation where appropriate.
- Deploy ensemble and boosting models in production for best-in-class risk stratification.
- Develop real-time scoring APIs.

---

## Setup and Installation

**Requirements**
- Python 3.7+
- Key packages: numpy, pandas, scikit-learn, imbalanced-learn, matplotlib, seaborn, xgboost, jupyter


**Install dependencies**

pip install -r requirements.txt

**Usage**
git clone https://github.com/mimomaina/Machinelearning001.git

cd Machinelearning001/Credit_risk_modelling

Place your data CSV as 'raw-data.csv' in this directory


---

## License and Contact

Distributed under the MIT License.

Maintainer: [mimomaina](https://github.com/mimomaina)

For questions or issues, please open an Issue in this repository.

---

This README covers data preparation, model building, insights from visuals and evaluation metrics, as well as practical business usage for risk scoring and deployment.

jupyter notebook Credit_risk_modelling.ipynb
