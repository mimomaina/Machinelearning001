# Blueberry Yield Prediction

## Project Overview

This project aims to predict the yield of wild blueberries using a dataset that simulates real-world agricultural conditions. The dataset includes environmental, pollination, and biological variables collected across **777 simulated field observations**. The primary objective is to build a regression model that accurately predicts the target variable, yield, based on the provided features. Performance is evaluated using **Mean Absolute Error (MAE)**, which serves as the official metric for the Kaggle competition: _Blueberry Yield – Zindua ML Week 2 Challenge_[1].

The solution leverages regression techniques including **Linear Regression, Ridge, Lasso, and ElasticNet**. Models are trained and validated within a consistent pipeline, and the final output consists of predictions on the test dataset, formatted as a Kaggle-compatible submission.

---

## Dataset Description

- **Files:** `train.csv`, `test.csv`
- **Train Set:** 777 rows × 18 columns (17 features + 1 target `yield`)
- **Test Set:** Same features, without `yield` (requires prediction)

**Features:**
- `id` — Unique row identifier (used for submission)
- `clonesize` — Area of blueberry clone (m²)
- `honeybee`, `bumbles`, `andrena`, `osmia` — Pollinator density (bees/area/day)
- `MaxOfUpperTRange`, `MinOfUpperTRange`, `AverageOfUpperTRange` — Upper canopy temperatures
- `MaxOfLowerTRange`, `MinOfLowerTRange`, `AverageOfLowerTRange` — Lower canopy temperatures
- `RainingDays`, `AverageRainingDays` — Rainfall metrics
- `fruitset` — Proportion of flowers that set fruit
- `fruitmass` — Average fruit mass (g)
- `seeds` — Average seeds per fruit
- `yield` — Target output (arbitrary units)

_All features are numeric; no missing values in train/test. The `id` column is excluded from modeling but used for submission alignment._

---

## Exploratory Data Analysis

- **Yield distribution:** Normal, mean ≈ 6000 units, std dev ≈ 1500 units (range: ~2000–12000)
- **Correlations:**
  - **Strong positive:** `fruitset`, `fruitmass`, `seeds` with `yield`
  - **Moderate positive:** Pollinators (`honeybee`, `bumbles`) with `yield`
  - **Weak associations:** Temperature features (values mostly optimal)
  - **Mild negative:** Rainfall variables (too much rain can hinder yield)
- **Multicollinearity:** Low VIFs support linear model use and coefficient interpretability[1].

---

## Methodology

1. **Preprocessing:** 
   - Column names standardized.
   - Remove `id` from features (preserved for submission).

2. **Train/Test Splitting:**
   - 80% train, 20% validation (`random_state=42` for reproducibility).

3. **Pipeline:**
   - **StandardScaler** (feature normalization)
   - Regression model (selectable)

4. **Models Evaluated:**
   - **Linear Regression** (baseline)
   - **Ridge Regression** (L2 regularization)
   - **Lasso Regression** (L1 regularization)
   - **ElasticNet** (L1+L2 mix)

_Default parameters: Ridge α=1.0, Lasso & ElasticNet α=0.1. No extensive hyperparameter tuning in baseline._

---

## Model Training & Evaluation

| Model              | Validation MAE |
|--------------------|:--------------:|
| Linear Regression  |   **369.29**   |
| Ridge              |    369.71      |
| Lasso              |    370.56      |
| ElasticNet         |    377.62      |

- **Linear Regression** selected as best-performing model. Regularization (Ridge/Lasso) had minimal impact due to absence of strong multicollinearity and high signal-to-noise ratio. ElasticNet performed slightly worse, likely due to parameter choices.
- Final model retrained on **full train data**, then used for test set prediction.

---

## Results Interpretation

- Predicted yields on test set closely match training data distribution (center ≈ 6000 units, range ≈ 4000–8000).
- MAE of 369.29 ≈ 6.15% of mean yield—robust for agricultural prediction.
- **Submission file**: Two columns—`id` and `yield`—ready for Kaggle upload.

**Feature importance (Linear Regression):**
- Largest positive impact: `fruitmass`, `seeds`, `fruitset`
- Positive effect: `clonesize`
- Pollinators: `honeybee`, `bumbles` > `andrena`, `osmia`
- Temperature: Neutral/mild effects
- Rainfall: Mild negative impact

Interpretations align with agricultural science and validate model plausibility.

---

## Potential Improvements

- **Polynomial Features:** Model nonlinear relationships (e.g. temperature × pollinator effects)
- **Hyperparameter Tuning:** Use GridSearchCV/RandomizedSearchCV for Ridge/Lasso/ElasticNet
- **Feature Engineering:** Composite metrics (bee density, temp range, fruit efficiency)
- **Cross-Validation:** k-fold for robust MAE estimates
- **Advanced Models:** Try Random Forest, Gradient Boosting for non-linear effects

---

## Usage Instructions

**Prerequisites:**
- Python 3.7+  
- Libraries: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`


**Steps:**
1. Place `train.csv` and `test.csv` in your working directory.
2. Ensure Kaggle API is configured (`kaggle.json` auth token).
3. Run the notebook: `Blueberry_yield_prediction.ipynb`.
4. The output file `submission.csv` will be generated in Kaggle format.

---

## License

This project is shared under the MIT License. You are free to use, modify, and distribute this work for educational or commercial purposes. Created for Zindua School of Technology and Innovation's Machine Learning program, Week 2: Linear Modeling.

---

## Contact

Maintainer: [mimomaina](https://github.com/mimomaina)  
Questions/issues: Please open an Issue in the repository.

---




