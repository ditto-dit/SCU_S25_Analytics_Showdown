# Objective 1 — Late Payment Risk Factors & Default Prediction

## Problem Statement

The client serves families in remote Indigenous communities who pay for their solar systems in monthly microcredit installments. When families fall behind, it threatens the enterprise's financial sustainability, ability to reinvest in growth, and trust within local communities.

**Goal:** Identify which behavioral and financial signals predict whether a customer will default on their payments, and generate individual risk scores that help Guardians intervene before it's too late.

---

## Tools & Libraries

| Tool | Purpose |
|------|---------|
| `pandas` | Data loading, merging, and aggregation |
| `NumPy` | Numerical operations |
| `Matplotlib` / `Seaborn` | Correlation heatmaps and visualizations |
| `scikit-learn` | Logistic Regression, Random Forest, model evaluation |
| `XGBoost` | Gradient boosting classifier (benchmarked) |

---

## Data Sources Used

The analysis merged **6 datasets** at the client level:

| Dataset | What it contributed |
|---------|---------------------|
| `client_data` | Default status, contract flags, guardian assignment |
| `repayment_data` | Payment history — lateness, amounts paid vs. due |
| `reconciled_payment_data` | Verified payment totals |
| `reversed_payments_data` | Count of cancelled/refunded payments per client |
| `interaction_data` | Number of guardian-client touchpoints |
| `user_data` | Guardian role and region |

---

## What Was Analyzed

### Step 1 — Feature Engineering
Raw transaction logs were aggregated to create client-level behavioral signals:

| Feature | Description |
|---------|-------------|
| `avg_days_late` | Average number of days payments were overdue |
| `max_days_late` | Worst single payment delay (most predictive) |
| `total_paid` | Total amount the client has paid to date |
| `total_due` | Total amount owed across all payment obligations |
| `num_payments` | Number of payment transactions made (engagement proxy) |
| `num_reversals` | Count of cancelled or reversed payments |
| `interaction_count` | Number of guardian-client interactions logged |

### Step 2 — Correlation Analysis
A heatmap revealed the strongest correlations with the `defaulted` target variable:

- `total_paid` had the **strongest negative correlation** with default (-0.59) — clients who pay more are far less likely to default
- `total_due` also correlated with default risk (+0.58) — higher total debt signals higher risk
- `num_payments` was negatively correlated (-0.57) — consistent payment history signals reliability
- `max_days_late` was more predictive of default than `avg_days_late`

### Step 3 — Model Comparison

Three classifiers were trained and compared:

| Model | Accuracy | Precision | Recall | F1 Score |
|-------|----------|-----------|--------|----------|
| Logistic Regression | 89.8% | 96.4% | 76.1% | 0.850 |
| **Random Forest** | **91.4%** | **93.7%** | **83.1%** | **0.881** |
| XGBoost | 91.4% | 95.1% | 81.7% | 0.879 |

**Random Forest was selected** as the final model for its balance of accuracy and interpretability via feature importance rankings.

### Step 4 — Default Probability Scoring
Using `.predict_proba()`, each of the 932 clients was assigned a **default risk score between 0 and 1**. The top 10 highest-risk clients all scored at or near 1.0, allowing guardians to prioritize outreach with precision.

---

## Key Insights

1. **Payment volume matters most** — how much a client has paid is the single strongest signal of whether they'll default. Clients who've paid more are significantly safer.
2. **Worst delay beats average delay** — a single extreme late payment is more predictive than an average across all payments. A client who was 265 days late once is a bigger risk than one who is consistently 15 days late.
3. **Engagement reduces risk** — clients with more guardian interactions and more payments on record are less likely to default.
4. **A risk score, not just a label** — outputting a probability (e.g., 0.82 vs. 0.41) enables graduated response rather than binary intervention.

---

## Recommendations

- **Monthly scoring pipeline:** Re-score all active clients each month using updated payment data; automatically flag anyone above a 0.75 threshold.
- **Onboarding risk assessment:** At sign-up, collect basic indicators (income, family size, proximity to prior defaulters) to assign early engagement intensity.
- **Guardian dashboards:** Visualize client risk in green/yellow/red bands so guardians can prioritize their weekly outreach.

---
