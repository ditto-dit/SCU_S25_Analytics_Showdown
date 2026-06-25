# 🌞 Solar Energy — Analytics Showdown (2nd Place)
**SCU Analytics Showdown | Group 4**

> **Award:** 🥈 2nd Place — SCU Analytics Showdown

---

## Project Overview

**The client** is a Colombian social enterprise that brings solar energy to remote Indigenous communities through a microcredit model — families pay for their solar systems in monthly installments managed by local community agents called *Guardians*.

Our team was given over **48,000 records** spanning payments, contracts, client profiles, and geographic data. The challenge: turn raw data into actionable insights that help the client reduce payment defaults, predict disconnection risks, and identify their strongest and weakest performing zones and agents.

---

## The Three Objectives

| # | Objective | Description |
|---|-----------|-------------|
| 1 | **Late Payment Risk Factors** | Identify what drives families to fall behind on payments and predict default risk |
| 2 | **Disconnection Risk Prediction** | Flag customers who may be experiencing solar system connectivity issues |
| 3 | **Performance Benchmarking** | Compare Guardian agents and villages to find top and bottom performers |

---

## Tools & Technologies

| Category | Tools Used |
|----------|-----------|
| **Language** | Python 3 |
| **Data Manipulation** | pandas, NumPy |
| **Visualization** | Matplotlib, Seaborn |
| **Machine Learning** | scikit-learn (Random Forest, Decision Tree, Logistic Regression, XGBoost) |
| **Environment** | Jupyter Notebook / Google Colab |

---

## Dataset Overview

The dataset covered **932 clients** across **31 villages** in rural Colombia, with data from multiple interrelated tables:

- `clients` — client profiles and contract statuses
- `repayment_data` — payment transaction history with lateness flags
- `reconciled_payments` — verified/confirmed payments
- `reversed_payments` — refunded or cancelled payment records
- `contracts` / `contract_events` — installation and service timelines
- `users` — Guardian (agent) profiles and roles
- `village_data` — geographic and operational entity mapping
- `interactions` — client-guardian engagement logs
- `product_subtypes` — solar device model information

---

## Key Results Summary

### Objective 1 — Payment Default Prediction
- Built a **Random Forest classifier** achieving **91% accuracy** and an **F1 score of 0.88**
- The most predictive features were total amount paid, number of payments made, and worst single payment delay
- Generated per-client default probability scores (0–1) to prioritize guardian outreach

### Objective 2 — Disconnection Risk Detection
- Engineered a "Possible Connectivity Issue" label using behavioral logic (inactive contracts with no defaults and no delinquencies)
- Built a **Decision Tree model** achieving **83% accuracy**, with 100% recall on connectivity-at-risk cases
- Found that location (specifically the village of Kainnamana) was the strongest predictor of connectivity issues

### Objective 3 — Performance Benchmarking
- Ranked all 31 villages and 7 Guardian agents across four KPIs: revenue per client, on-time payment rate, retention rate, and lead conversion rate
- **Best village:** Wikunmake (ranked #1 across all KPIs)
- **Worst village:** Karapashen (ranked #31, with only 25.6% client retention)
- **Best guardians:** User IDs 16, 14, and 9 (all Agents) with the lowest late-payment rates and highest retention
- All guardians achieved **100% lead conversion rates** and **≥100% payment collection rates**

---

## Strategic Recommendations

1. **Early Risk Detection System** — Score all active clients monthly; flag those above 0.75 default probability for proactive guardian outreach
2. **Proactive Guardian Outreach** — Equip guardians with risk band dashboards (green/yellow/red) and templated visit guides for high-risk clients
3. **Client Risk Profiling at Onboarding** — Use income indicators, family size, and proximity to past defaulters to assign engagement strategies from day one
4. **Targeted Expansion Planning** — Prioritize new installations in low-default zones (e.g., Bukuamake, Sabanatico); allocate extra support to high-risk regions
5. **Role-Based Guardian Guidelines** — Reinforce what Agents do well; investigate how Admin and ViewOnly users are assigned, as they show weaker retention outcomes
6. **Connectivity Monitoring** — Investigate Kainnamana's recurring disconnection pattern; introduce periodic customer connectivity surveys and real-time logging
7. **Audit Lead Conversion Metrics** — Investigate and validate the near-perfect conversion rates before using them to drive resourcing decisions (see below)
8. **Invest in Qualitative Data Collection** — Pair the behavioral models with family interviews, guardian field notes, and community mapping to capture the relational dynamics that numbers alone cannot reflect

---

## Judges' Questions & Our Responses

This project was presented to a panel of judges at the SCU Analytics Showdown. Their questions pushed us to think beyond the models and consider the human and cultural dimensions of the data.

### Q1: Did you have enough information about villages and their context? There could be important variables behind them.

The dataset gave us village-level identifiers and names, but limited socioeconomic or geographic features — no income levels, infrastructure access, education data, or cultural context. We worked around this by aggregating default rates and repayment behavior by region and guardian zone, which gave us some signal on performance variation across areas.

But we agree: more contextual village-level data could unlock hidden drivers of default risk, such as migration patterns, distance from markets, or access to financial services. The ideal next step would be to combine our current behavioral model with geospatial and socioeconomic indicators to build a geospatially-aware risk model. Village population data alone would already improve lead generation targeting by revealing market size potential.

### Q2: What do you think about the abnormally high Lead Conversion Rates? Could they have future effects?

The near-100% conversion rates across all guardians are a red flag worth investigating. While they look like strong performance on the surface, they may be masking real risks:

- **Metric inflation:** Guardians may be marking leads as "ready to buy" before contracts are signed or payments are secured, inflating the metric and setting up future cancellations.
- **Narrow lead pools:** High conversion may reflect guardians focusing on warm leads — friends, family, close community members — rather than engaging broader markets. Once personal networks are exhausted, growth stalls.
- **Incentive distortion:** If guardian incentives are tied to conversion counts, premature classification becomes a rational (but harmful) behavior.

If left unexamined, this could cause the client to misallocate resources toward hiring more agents to convert leads, rather than toward marketing and outreach to find new ones. It could also prevent agents from developing the broader sales skills they'd need for sustainable growth. We recommend clearly defining what "converted" means, tracking lead source channels, and running periodic quality audits on conversion classifications.

### Q3: These communities are living systems — complex, dynamic, and relationship-based. What qualitative dimensions emerge from your analysis? What intangible characteristics define the most successful guardians and communities?

This question resonated deeply with us. The data gave us numbers, but the numbers pointed to something more human underneath.

We observed, for example, that some guardians had strong repayment performance but modest conversion rates — suggesting they may prioritize trust and long-term engagement over quick sign-ups. Some regions with similar revenue per contract had vastly different default rates, hinting at differences in social cohesion, trust in guardians, or cultural attitudes toward credit and financial commitment.

The intangible qualities that likely define the best guardians — reliability, trustworthiness, active listening, cultural familiarity — simply don't appear in a dataset. Nor does the lived experience of what access to electricity means for a family: whether it enables children to study at night, powers a small business, or just brings a sense of dignity and stability.

To capture this, the client could invest in:
- **Family interviews** — to understand what challenges electricity solves and what barriers remain
- **Guardian field notes and comments** — qualitative observations logged alongside numerical data
- **Community mapping** — charting influence networks and the relationships between guardians and clients
- **Household financial behavior tracking** — capturing seasonal income pressures that drive late payments
- **Perception surveys** — measuring whether clients find guardians helpful, accessible, and trustworthy

The data models we built are a starting point, not the whole picture. The strongest version of this work would pair predictive analytics with ethnographic insight — treating these communities as the living systems they are.

---

## Team

**Group 4 — SCU Analytics Showdown**

*Detailed analyses for each objective are in the `/notebooks` folder. The full presentation deck is in `/presentation`.*
