# Objective 3 — Performance Benchmarking: Villages & Guardians

**Note:** This README summarizes the work completed across two separate notebooks: **Part A (Guardian Performance Analysis)** and **Part B (Village Performance Analysis)**. Results, methodologies, and findings from both notebooks are consolidated here for a complete view of Objective 3.

## Problem Statement

The client operates across **31 villages** and manages clients through a network of community agents called **Guardians**. Leadership needs a clear, data-driven picture of where collection and client behavior are strongest — and where intervention is needed.

**Goal:** Build a set of KPIs to rank villages and Guardian agents, surface the top and bottom performers, and identify patterns that explain the performance gaps.

---

## Tools & Libraries

| Tool | Purpose |
|------|---------|
| `pandas` | Data merging, aggregation, and KPI calculation |
| `NumPy` | Ranking and numerical operations |
| `Matplotlib` / `Seaborn` | Revenue-over-time charts, late payment trend lines |

---

## Data Sources Used

| Dataset | What it contributed |
|---------|---------------------|
| `repayment_data` | Days late, payment amounts, on-time vs. late flags |
| `client_data` | Contract statuses, default flags, guardian assignment |
| `user_data` | Guardian IDs, roles, and entity assignments |
| `village_data` / entity tables | Village-level geographic grouping |
| `leads_data` | Lead counts per guardian for conversion rate calculation |

---

## Part A — Guardian Performance

### KPIs Defined

| KPI | Definition | Why It Matters |
|-----|-----------|---------------|
| **Average Payment Delay** | Mean days a payment is made after the due date (negative = paid early) | Shows how well a guardian keeps clients accountable |
| **Payment Collection Rate** | Actual payments collected ÷ expected payments | Measures revenue recovery effectiveness |
| **Lead Conversion Rate** | Leads converted to "installed" or "ready to buy" ÷ total leads | Reflects the guardian's sales ability |
| **Client Retention Rate** | Active clients ÷ total clients (excludes defaults, cancellations, repossessions) | Indicates long-term client satisfaction and engagement |

### Results

**Overall average payment delay: −11.6 days** — meaning payments are typically received nearly two weeks *before* the due date across the portfolio.

**Payment Collection Rate:** All 7 guardians achieved **100% or above** — some collected more than expected due to clients paying early or in lump sums.

**Lead Conversion Rate:** All guardians achieved **94–100% conversion**, with the majority at a perfect 100%.

#### Guardian Rankings (Combined KPIs)

| Guardian ID | Role | Late Payment % | Collection Rate | Retention Rate | Overall |
|-------------|------|----------------|-----------------|----------------|---------|
| 16 | Agent | 29.95% | 100.0% | 72.9% | **Best** |
| 14 | Agent | 60.44% | 100.0% | 74.3% | **Best** |
| 9 | Agent | 61.33% | 100.03% | 71.9% | **Best** |
| 7 | Admin | 62.60% | 100.0% | 54.4% | Mid |
| 1 | SuperAdmin | 49.74% | 100.04% | 49.4% | Weak |
| 5 | ViewOnly | 65.96% | 100.10% | 45.5% | Weak |
| 10 | Agent | 75.00% | 100.0% | 27.3% | **Worst** |

### Key Insight on Roles
Agent-role guardians dominate the top performers. Admin, ViewOnly, and SuperAdmin roles show notably weaker client retention, suggesting a mismatch between system access roles and field accountability.

---

## Part B — Village Performance

### KPIs Defined

| KPI | Definition | Why It Matters |
|-----|-----------|---------------|
| **Revenue Per Client** | Total revenue collected ÷ number of contracts | Normalizes revenue by village size |
| **On-Time Payment %** | On-time payments ÷ all payments | Measures financial discipline in the community |
| **Client Retention Rate** | Non-defaulted contracts ÷ all contracts | Tracks long-term commitment to the program |
| **Lead Conversion Rate** | Converted leads ÷ total leads | Reflects community uptake and guardian effectiveness |

### Top 5 Villages

| Village | Avg Revenue/Client | On-Time % | Retention | Lead Conversion | Rank |
|---------|-------------------|-----------|-----------|----------------|------|
| **Wikunmake** | $4,795,515 | 78.06% | 77.78% | 100% | 🥇 1 |
| Juan de Aragón | $5,909,104 | 66.28% | 83.33% | 100% | 2 |
| Jonchon | $7,105,625 | 57.72% | 83.33% | 100% | 3 |
| El Colorado | $6,582,623 | 54.42% | 74.29% | 100% | 4 |
| Wirmana | $5,071,481 | 58.00% | 51.85% | 100% | 5 |

### Bottom 5 Villages

| Village | Avg Revenue/Client | On-Time % | Retention | Lead Conversion | Rank |
|---------|-------------------|-----------|-----------|----------------|------|
| Kawayanse | $2,049,182 | 45.90% | 45.45% | 100% | 27 |
| Limón Carrizal | $1,343,571 | 68.18% | 14.29% | 87.5% | 28 |
| Jasaishao | $3,190,000 | 48.33% | 54.55% | 91.67% | 29 |
| Sebowawimake | $1,760,000 | 38.97% | 33.33% | 100% | 30 |
| **Karapashen** | $2,295,465 | 38.97% | 25.58% | 95.56% | 🔴 31 |

### Notable Patterns Found During Exploration

- **New contract surge in July 2024:** Nearly all new contracts were signed in a single month, creating a data spike that drops sharply afterward.
- **Revenue spike then stabilization:** All villages saw a large revenue peak in July 2024 (tied to contract starts and down payments), which then normalized.
- **Steep losses in some villages:** Certain villages showed sharp revenue drops tied to payment reversals and down payment reimbursements — indicating early-stage churn.

---

## Key Insights

1. **Wikunmake is the model village** — highest on-time payment rate (78%) combined with strong retention and perfect lead conversion.
2. **Karapashen needs urgent attention** — only 25.6% of clients have retained their contracts, and the on-time payment rate is the second-lowest across all villages.
3. **Agents outperform all other roles** — the top three guardians are all Agents; retention falls significantly for Admin and ViewOnly role-holders.
4. **Revenue per client ≠ overall performance** — Jonchon has the highest average revenue per client but ranks only 3rd overall because its on-time payment rate lags behind Wikunmake.
5. **Lead conversion is nearly universal** — this is a strength across the entire network; the challenge lies in *keeping* clients, not acquiring them.

---

## Recommendations

- **Replicate Wikunmake's practices** — conduct qualitative research to understand what guardian behaviors or community dynamics make it the strongest performer.
- **Intervention plan for Karapashen** — assign the highest-performing guardian to this village or provide additional support resources.
- **Role-based guidelines for guardians** — formalize what Agents do well and build training modules for Admin/ViewOnly users who show lower retention outcomes.
- **Audit lead conversion metrics** — before using conversion rates to drive hiring or resourcing decisions, validate how "converted" is being defined and tracked.
- **Continue lead generation in stable zones** — given the near-perfect conversion rates in well-performing villages, expanding outreach there is a lower-risk growth path than pushing into high-churn areas.

---
