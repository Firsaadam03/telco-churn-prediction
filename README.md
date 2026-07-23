<h1> Telco Customer Churn Prediction </h1>

## 1. Project Overview

This project analyzes telco (telecommunications) customer data to predict customer churn and support proactive retention strategy. The primary focus is to **identify customers at high risk of churning before they leave, and estimate the business impact of a model-driven retention program** — since retaining existing customers is significantly cheaper than acquiring new ones in a subscription-based business.

The company's historical churn rate (26.69%) is higher than the telecom industry benchmark (~21.5–22%), which motivated this project: to move from reactive retention (acting after a customer has already left) to proactive retention (intervening before churn happens), using a classification model trained on customer attributes.

Key Objectives:

* Objective 1: Identify customer characteristics (contract type, add-on services, tenure, monthly charges, etc.) most associated with high churn risk.
* Objective 2: Build a classification model that predicts churn probability per customer, so CRM/Marketing teams can prioritize retention efforts.
* Objective 3: Translate model predictions into an estimated business impact (potential retained revenue), to support the retention program's business case.

## 2. Data Sources

- `data_telco_customer_churn.csv` — Telco customer records (4,930 rows, 11 columns), including customer demographics, subscribed services, contract type, billing information, and churn status (target).

## 3. Technologies Used

* Programming Language: Python (pandas, numpy)
* Machine Learning: scikit-learn, imbalanced-learn (SMOTE, RandomOverSampler), XGBoost, LightGBM
* Statistical Analysis: statsmodels (VIF), scipy (Cramer's V)
* Model Interpretation: SHAP
* Visualization: Matplotlib, Seaborn
* Model Persistence: joblib
* Version Control: Git
* Others: Jupyter Notebook

## 4. Project Structure

```
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump (data_telco_customer_churn.csv).
│
├── models             <- Trained and serialized models (model_churn_adaboost.pkl)
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-bdp-notebook`.
│
├── references          <- Data dictionaries, manuals, and all other explanatory materials
│                          (external_references.txt).
│
├── reports             <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures         <- Generated graphics and figures used in reporting (EDA plots, SHAP plots, etc.)
│
├── requirements.txt    <- The requirements file for reproducing the analysis environment.
│
└── src                 <- Source code for use in this project.
```

## 5. Summary of Finding

### 5.1 Business Insight

* **Contract type, tenure, and internet service type** are the three strongest predictors of churn, consistent across EDA, feature importance, and SHAP analysis.
* Customers on **Month-to-month contracts** churn at a much higher rate (43.2%) than those on One year (10.1%) or Two year (2.8%) contracts.
* **New customers (low tenure)** are far more likely to churn than long-tenured customers - median tenure for churned customers is 10 months, vs. 39 months for retained customers.
* Customers subscribed to **Fiber optic internet** churn more (41.9%) than DSL (18.3%) or no-internet customers (7.5%), suggesting potential issues with pricing or service quality for this segment.
* The combination of **Month-to-month contract + Fiber optic** is the highest-risk segment, with a churn rate of 54.6% - substantially higher than either factor alone.
* The final model (AdaBoost: learning_rate=0.1, max_depth=1, n_estimators=100, with RandomOverSampler) achieves **85.7% Recall** and **0.832 ROC-AUC** on unseen test data, exceeding the project's success criteria (Recall > 75%, ROC-AUC >= 0.80), with minimal train-test gap (<2% across all metrics), indicating no overfitting.
* Simulation on test data shows that even under conservative retention program assumptions (10% success rate), the model's use has a **net positive financial impact** ($14,870/year), with churn rate reduction potential from 26.57% down to 17.51%-24.30% depending on retention program effectiveness - meeting or exceeding the industry benchmark of ~22%.

### 5.2 Actionable Recommendation

* **CRM Team** : Use the model's churn probability score to segment customers into Low (0-0.358) / Medium (0.358-0.575) / High (0.575-1.0) risk tiers and prioritize outreach to High Risk customers first; validate these thresholds against actual operational capacity.
* **Marketing Team** : Target retention campaigns (e.g., contract upgrade incentives) specifically at customers on Month-to-month contracts, especially those also on Fiber optic - the highest-risk segment identified. Counterfactual analysis shows upgrading a high-risk customer's contract to Two year can reduce their individual churn probability by ~24 percentage points.
* **Product Team** : Investigate the root cause of higher churn among Fiber optic subscribers (pricing, service quality, or expectations mismatch), and consider structural incentives to migrate customers from Month-to-month to longer-term contracts.
* **General** : Re-evaluate model performance quarterly; retrain if ROC-AUC drops below 0.75, or if customer population characteristics shift significantly (data drift).

## 6. Contact

- Name:
- Email:
- Linkedin:
