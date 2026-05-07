# Fraud Detection & Cost Optimization

This project simulates a real-world fraud detection system, focusing on **cost-sensitive decision-making** rather than pure model accuracy.

The **goal** is to identify fraudulent transactions while optimizing the trade-off between:
* Fraud losses (false negatives)
* Customer friction (false positives)

In real-world fraud systems, selecting the right decision threshold is as important as building the model itself.

To run this project, download the dataset and place it locally, then run the notebook.

**Dataset:**

* This project explores fraud detection on the publicly available [**Credit Card Fraud Detection dataset**](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) (MLG-ULB), with a focus on **cost-sensitive decision-making** rather than pure model performance.
* 284,807 transactions, highly imbalanced (fraudulent ~0.17%)
* Features are anonymized PCA components, plus `Amount` and `Time`.
* No access to raw behavioral features (e.g., user history, merchant, device). Therefore, feature engineering is limited; in real-world systems, additional behavioral and temporal features would significantly improve performance.

---

## Approach

### 1. Exploratory Data Analysis

* Analyzed class imbalance and feature distributions
* Compared feature behavior between fraudulent and legitimate transactions
* Identified potentially informative components

---

### 2. Modeling

* Trained classification models (Logistic Regression, Gradient Boosting)
* Used **stratified cross-validation** for robust evaluation
* Focused on stability and generalization

---

### 3. Cost-Sensitive Optimization and Evaluation Strategy (Key Contribution)

The goal is to find the combination of model and threshold that minimizes the average per-transaction cost taking into account:

* Precision–Recall trade-offs
* Impact of class imbalance
* Model stability across folds

The choice of the model and threshold was performed using k-fold validation using the per-transaction cost at minimum, balancing fraud detection and false positives. For each fold, the dataset is split in three components, one used only to train the model, one used only to select the threshold, the last one used to assess the cost due to both the threshold and the model. The model and threshold which give the lowest average cost and/or standard deviation are chosen. Finally, the per-transaction cost, the true positive rate and the false positive rate are computed using an holdout dataset.

---

## Final Decision

* Selected model: Logistic Regression (chosen for stability and interpretability)
* Decision threshold: optimized using cost-based objective. 0.957 found to minimize cost, with a true positive rate of 81% and a false positive rate of 0.2%
* Strategy: prioritize reduction of high-impact fraud while controlling false positives

---

## Business Impact Estimation

Using the optimized threshold, the expected cost was extrapolated to a yearly basis, resulting in an estimated loss on the order of millions of dollars. This estimation assumes the sample of transactions is representative of the typical yearly transaction volume for European cardholders, though it likely represents only a subset of all European cardholder transactions. At a larger scale—such as major financial institutions with significantly higher daily transaction volumes—this cost could grow substantially. For example, while this dataset covers just two days' worth of transactions, institutions processing millions of transactions daily could face even higher potential costs from false positives and false negatives.

---

## Limitations

* Dataset anonymization prevents behavioral feature engineering
* No user-level or temporal aggregation features
* Static dataset (2 days of data, no concept drift or real-time constraints)

---

## Future Improvements

* Incorporate behavioral features (e.g. transaction velocity, user-level aggregates)
* Compare against rule-based baselines
* Investigate how to reduce fluctuations and reduce cost (for example, probability calibration, assess sensitivity of probabilities to threshold, etc.)
* Extend to real-time decision systems with monitoring for concept drift

---

## Tech Stack

* Python (Pandas, NumPy, Scikit-learn)
* Jupyter Notebook

