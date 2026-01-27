# Credit Card Default Prediction

## Project Overview

This project demonstrates a complete machine learning pipeline for predicting credit card default risk using the **Default of Credit Card Clients** dataset from Taiwan (UCI ML Repo). The workflow includes robust data cleaning, exploratory analysis, feature engineering, handling class imbalance, model training, threshold adjustment, and final evaluation for practical risk management.

---

## 1. Dataset

- **Source:** [UCI ML Repo - Default of Credit Card Clients](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients)
- **Domain:** Credit risk, customer default prediction
- **Size:** 30,000 rows, 23 features + target
- **Target:** `default payment next month`
    - 1: Defaulted next month
    - 0: No default

### Main Features

- **Demographics:** Age, Sex, Education, Marriage
- **Credit history:** Payment status for previous 6 months (`PAY_0` to `PAY_6`)
- **Financials:** Credit limit, bill amounts, payments made

---

## 2. Data Preparation

### Cleaning & Transformation

- Removed or consolidated ambiguous values for `Education` and `Marriage`.
- Encoded categorical variables:
    - `SEX`: male/female -> binary
    - `EDUCATION` & `MARRIAGE`: one-hot encoding
- No missing values.

### Outlier Handling

- Negative and anomalous bill amounts clipped to zero.
- Log-transform applied to heavily skewed financial columns for stability.

### Feature Engineering

- **Ratio features:** Utilization ratio, payment ratio, balance ratio.
- **Trend features:** Change in bill/payments across months.
- **Consistency:** Average and variability of payment status.

---

## 3. Exploratory Data Analysis (EDA)

- **Imbalance:** 22% default, 78% non-default.
- **Strongest predictors of default:**
    - Payment delays (`PAY_0` to `PAY_6`)
    - Lower credit limit
    - Small/irregular payments
- **Demographic features have weak correlation** with default status.
- **Engineered features** (ratios, trends) improve signal.

---

## 4. Modelling Approach

- **Splitting:** Stratified train/test (80:20)
- **Scaling:** Standardization of numeric columns
- **Imbalance Handling:** Class weights for minority class (defaulters)

### Models Tested

| Model                | Accuracy | Precision | Recall | F1 | ROC-AUC |
|----------------------|----------|-----------|--------|----|---------|
| Logistic Regression  | 0.73     | 0.43      | 0.63   |0.51| 0.74    |
| Random Forest        | 0.82     | 0.67      | 0.34   |0.45| 0.76    |
| Gradient Boosting    | 0.82     | 0.67      | 0.37   |0.48| 0.78    |

**Best model:** Gradient Boosting offers a practical trade-off between recall and precision for risk management. Final metrics at threshold 0.24:
- **Accuracy:** 0.78
- **Precision (defaulters):** 0.50
- **Recall (defaulters):** 0.60
- **F1:** 0.54

#### Threshold tuning:
- Lowering default probability threshold improves recall (captures more defaulters), which is crucial for banks to minimize losses, even if precision drops.

---

## 5. Feature Importance

Top features (by Gradient Boosting):
- `PAY_0`, `AVG_PAY_STATUS`, `PAY_VARIABILITY`, `BILL_AMT1`, `LIMIT_BAL`, `PAY_2`, `BILL_TREND`, `UTILIZATION_RATIO`

---

## 6. Practical Insights

- **Delayed payments** and **credit limit** are key risk factors.
- **Engineered features** provide additional signal above raw columns.
- **Balancing precision/recall by adjusting threshold** allows tailoring the model to match business tolerance for false alarms vs. missed defaults.
- **Gradient Boosting** gives strong performance out-of-the-box, and can be fine-tuned further using hyperparameter search.

---

## 7. How to Run

**Requirements**
- Python 3.7+
- Packages: numpy, pandas, scikit-learn, matplotlib, seaborn, xgboost

**Setup**
git clone https://github.com/mimomaina/Machinelearning001.gits

cd Machinelearning001/Default_of_Credit_Card_Clients

Download the dataset from the UCI link above, place the .xls file here

jupyter notebook Default_of_Credit_Card_Clients.ipynb


---

## 8. License & Reference

- Dataset: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- Creator: I-Cheng Yeh (Expert Systems with Applications, 2009)
- Maintainer: [mimomaina](https://github.com/mimomaina)

---
