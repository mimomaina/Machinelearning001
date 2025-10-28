# Credit Card Fraud Detection

## Project Overview

This project implements a machine learning pipeline to detect fraudulent credit card transactions in a highly imbalanced financial dataset. It demonstrates best practices for data preparation, handling class imbalance, model building, and rigorous evaluation using metrics well-suited for fraud detection.

Credit card fraud detection is essential for financial institutions to minimize losses and protect customers. The dataset consists of 284,807 transactions of European cardholders over two days (September 2013). Only 492 transactions are fraudulent (about 0.17%), highlighting the severe class imbalance. All features except `Time`, `Amount`, and `Class` are anonymized (PCA components V1–V28) for security.

---

## Workflow Steps

### 1. Exploratory Data Analysis (EDA)
- Confirmed absence of missing values.
- Feature variance: `Time` and `Amount` have much higher variance compared to anonymized components. Higher-variance components (e.g., V1–V11) tend to capture meaningful patterns important for separating fraud from non-fraud.
- Correlation visualization: Most `V` columns have minimal correlation with each other, ensuring information is distributed with little redundancy and reducing risk of multicollinearity.
- Time-based grouping: Created hourly and 30-minute intervals to study transaction volume and fraud occurrences over time. Fraud rates fluctuate and are not strictly dependent on transaction volume.
- Boxplots: Fraudulent transactions often involve higher amounts, but there is significant overlap with legitimate transactions, so amount alone is insufficient for detection.

### 2. Preprocessing
- Train/test split with stratification to preserve class distribution.
- Applied RobustScaler to `Time` and `Amount` to reduce effects of outliers, which are common in financial transaction data.
- Used SMOTE (Synthetic Minority Oversampling Technique) on training data only to create a balanced training set (1:1 fraud/non-fraud), which prevents models from ignoring rare fraud cases.

### 3. Modeling
- Logistic Regression: Baseline linear model, strong at identifying most fraudulent cases (high recall), but less precise (many false alarms).
- Random Forest: Ensemble model, achieves better precision-recall balance and fewer false positives—suited for real-world deployment where minimizing false alerts is critical.
- Models are evaluated on the original imbalanced test set to simulate production conditions, not on balanced/oversampled data.

---

## Evaluation Metrics and Visual Interpretation

Metrics chosen for fraud detection must account for extreme imbalance:

- Precision: Fraction of predicted frauds that are truly frauds. High precision means fewer false positives.
- Recall: Fraction of all true frauds detected. High recall means most frauds are identified.
- F1 Score: The harmonic mean of precision and recall.
- AUPRC (Area Under Precision-Recall Curve): Comprehensive measure when the positive class (fraud) is a small minority.

### Model Performance Table

| Model              | Precision | Recall  | F1 Score | AUPRC  |
|--------------------|:---------:|:-------:|:--------:|:------:|
| Logistic Regression|  0.0785   | 0.8581  | 0.1438   | 0.7396 |
| Random Forest      |  0.8722   | 0.7838  | 0.8256   | 0.8251 |

- Logistic Regression: Yields high recall, capturing most actual fraud cases, but has low precision, meaning many flagged transactions are legitimate. This model is suitable when catching all fraud is paramount and manual review capacity is high.
- Random Forest: Achieves higher precision and a strong recall, offering a better balance for automated fraud detection—fewer unnecessary alerts and less strain on manual review.

**Visual Insights:**
- Precision-Recall Curves: The Random Forest curve stays higher across recall values, signaling reliable fraud alerts; Logistic Regression’s curve drops sharply, showing many alerts are not true fraud.
- Confusion Matrices: Random Forest leads to significantly fewer false positives compared to Logistic Regression.
- Transaction Amount Boxplots: Fraudulent transactions are more likely to involve larger amounts, but legitimate high-value transactions are common, so amount alone can’t be used to flag fraud.

---

## Conclusions and Next Steps

- Applying SMOTE to training data is essential; do not balance your test set, as fraudulent transactions are rare in reality.
- Random Forest provides superior performance for most practical scenarios.
- Further improvements can include:
  - Using boosting algorithms (XGBoost, LightGBM)
  - Model stacking or ensemble approaches
  - Engineering additional time-based features or transaction frequency metrics
  - Packaging the solution as an API for real-time deployment

---

## Setup and Installation

**Requirements**
- Python 3.7+
- Required packages (in requirements.txt):
  -numpy
  -pandas
  -scikit-learn
  -imbalanced-learn
  -matplotlib
  -seaborn
  -jupyter

  
**Install dependencies**

pip install -r requirements.txt


**Usage**

git clone https://github.com/mimomaina/Machinelearning001.git

cd Machinelearning001/Credit_Card_Fraud_Detection


---

## License and Contact

Distributed under the MIT License.

Maintainer: [mimomaina](https://github.com/mimomaina)

For questions or issues, please open an Issue in this repository.

---

This README covers the complete workflow, code, evaluation, and interpretation of results. It is formatted for direct pasting into GitHub and provides a robust starting point for further work in credit card fraud detection.



