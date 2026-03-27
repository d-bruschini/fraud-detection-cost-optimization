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

### 3. Evaluation Strategy

Instead of relying solely on ROC AUC, the project emphasizes:

* Precision–Recall trade-offs
* Impact of class imbalance
* Model stability across folds

---

### 4. Cost-Sensitive Optimization (Key Contribution)

A decision threshold was selected based on **expected business cost**, incorporating:

* Cost of missed fraud (false negatives)
* Cost of blocking legitimate users (false positives)

This reflects how fraud systems operate in practice:

> selecting operating points based on business impact, not just model scores.

---

## Results & Insights

* Model performance alone (e.g. ROC AUC) is not sufficient to determine the optimal decision strategy 
* Cost-based threshold optimization provides a more realistic framework for decision-making 
* Poor threshold selection can lead to suboptimal results, even with powerful models

A final operating point was selected based on minimizing expected cost, balancing fraud detection and false positives.

---

## Final Decision

* Selected model: Logistic Regression (chosen for stability and interpretability)
* Decision threshold: optimized using cost-based objective. 0.95 found to minimize cost, with a true positive rate of 82% and a false positive rate of 0.1%
* Strategy: prioritize reduction of high-impact fraud while controlling false positives

Note: the false positive and true positive rates are computed on the same dataset used for threshold optimization, and are therefore slightly optimistic.

---

## Business Impact Estimation

Using the optimized threshold, the expected cost was extrapolated to a yearly basis, resulting in an estimated loss on the order of millions of dollars.

This highlights that even well-performing models leave significant residual cost, due to the inherent trade-off between missed fraud and false positives.

At larger scale (e.g. major financial institutions with significantly higher transaction volumes), this cost can grow substantially, emphasizing the importance of continuous optimization and robust fraud strategies.

---

## Limitations

* Dataset anonymization prevents behavioral feature engineering
* No user-level or temporal aggregation features
* Static dataset (2 days of data, no concept drift or real-time constraints)

---

## Future Improvements

* Incorporate behavioral features (e.g. transaction velocity, user-level aggregates)
* Compare against rule-based baselines
* Optimize model and threshold jointly using cost-based metrics
* Extend to real-time decision systems with monitoring for concept drift

---

## Tech Stack

* Python (Pandas, NumPy, Scikit-learn)
* Jupyter Notebook

---

## Key Takeaways

This project demonstrates that in fraud detection:

* **Decision optimization under uncertainty and cost constraints is as important as model accuracy.**
* **Even well-performing models leave significant residual cost, due to the inherent trade-off between missed fraud and false positives.**

