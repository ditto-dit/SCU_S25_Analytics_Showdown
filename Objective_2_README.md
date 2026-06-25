# Objective 2 — Disconnection Risk Prediction

## Problem Statement

The client's solar systems are installed in remote communities with limited infrastructure. When a system stops working — whether from hardware failure, signal loss, or other issues — the family loses access to electricity. However, the dataset contains **no explicit "disconnected" label**, making this a problem that required creative feature engineering before any modeling could begin.

**Goal:** Identify customers whose systems may be experiencing connectivity issues, and build a model that predicts this risk based on location and product data.

---

## Tools & Libraries

| Tool | Purpose |
|------|---------|
| `pandas` | Data loading, merging, and label engineering |
| `NumPy` | Array operations and sparse matrix handling |
| `scikit-learn` | Decision Tree classifier, OneHotEncoder, class balancing |
| `Matplotlib` | Decision tree visualization |

---

## Data Sources Used

| Dataset | What it contributed |
|---------|---------------------|
| `client_data` | Contract status flags (active, completed, defaulted, late) |
| `contract_event_data` | Contract type and product sub-type per client |
| `village_data` / `L0 Entity Name` | Geographic location (village-level) |
| `product_subtypes_data` | Solar device model identifier |

---

## What Was Analyzed

### Step 1 — Engineering the "Possible Connectivity Issue" Label

Since no disconnection flag existed, the team defined a proxy label using a logical rule:

> A customer likely has a **connectivity issue** if they have:
> - An **inactive contract** (not currently active)
> - **No defaults** (they didn't stop paying due to financial failure)
> - **No delinquencies** (they weren't flagged as late-payers)
> - A contract that is **not completed** (the term isn't over)

This pattern — a client who stopped engaging despite being a good payer with an unfinished contract — is the strongest behavioral signal that something external (likely a hardware or connectivity issue) caused the disruption.

### Step 2 — Model Building

**Method:** Decision Tree Classifier
**Features:** Village (L0 Entity Name) and solar device model (Product Sub Type Id), both one-hot encoded
**Class Balancing:** Applied to handle the imbalance between connectivity-issue cases (minority) and normal cases

### Step 3 — Model Performance

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| 0 — No Connectivity Issue | 1.00 | 0.80 | 0.89 | 128 |
| 1 — Possible Connectivity Issue | 0.43 | **1.00** | 0.60 | 19 |
| **Overall Accuracy** | | | **83%** | 147 |

The model achieves **100% recall on connectivity-issue cases** — meaning it catches every at-risk customer, at the cost of some false positives. For this use case, missing a real issue is far more costly than investigating a false alarm, so high recall is the right priority.

### Step 4 — Decision Tree Interpretation

The tree revealed a clear geographic pattern in how it splits:
- **Kainnamana** as a village was the very first split point — clients in Kainnamana are disproportionately likely to have connectivity issues
- **Jasaishao** and **Bukuamake** appeared as low-risk branches — these villages have the most stable connections
- **Product sub-type** played a minor role at lower branches, suggesting device model is a secondary factor

---

## Key Insights

1. **Location is the primary driver of connectivity risk** — Kainnamana stands out as a hotspot for potential disconnections, pointing to geographic or infrastructure-specific challenges in that area.
2. **Jasaishao and Bukuamake are the most stable** — these villages can serve as benchmarks for infrastructure best practices.
3. **Device model matters, but less than location** — product sub-type influences risk at the margins, not at the core.
4. **100% recall is the right metric here** — it's better to over-investigate than to miss a family that has lost power.

---

## Recommendations

- **Prioritize Kainnamana** for field visits and infrastructure assessment.
- **Use the model's predictions monthly** to generate a watchlist of at-risk contracts for guardian follow-up.
- **Study the cost of disconnection** — understanding the revenue impact of each undetected disconnection would allow more efficient resource allocation.
- **Improve data infrastructure for future modeling:**
  - Introduce periodic customer connectivity surveys
  - Add real-time connectivity logging to the platform
  - Establish clear linkage rules between the payments, contracts, and device databases

---