# 📂 Dataset Documentation — Analytics Showdown

> ⚠️ **Note:** The raw data files are **not included** in this repository. The dataset was provided by the client exclusively for use in the SCU Analytics Showdown competition and contains sensitive client and financial information from real Indigenous communities in rural Colombia. All analysis was conducted in a secure, private environment.

This document describes the structure, contents, and purpose of each of the 19 datasets provided for the competition, so that readers can understand the scope and nature of the data used without accessing the underlying records.

---

## Dataset Overview

| File | Records | Description |
|------|---------|-------------|
| `client_data` | 932 | Client profiles |
| `lead_data` | 945 | Sales leads and acquisition pipeline |
| `lead_generator_data` | 21 | Community agents who generate leads |
| `user_data` | 25 | Guardian/agent accounts |
| `village_data` | 42 | Geographic village-level entities |
| `repayment_data` | 8,542 | Payment schedule and lateness tracking |
| `payments_data` | 10,898 | Raw payment transactions |
| `reconciled_payment_data` | 7,012 | Verified and reconciled payment records |
| `reversed_payments_data` | 349 | Cancelled or refunded payments |
| `contract_event_data` | 3,805 | Contract lifecycle events |
| `payment_wallets_data` | 967 | Client and agent payment wallets |
| `stock_data` | 940 | Solar device inventory |
| `stock_movements_data` | 1,030 | Device shipment and installation tracking |
| `offers_data` | 10 | Solar product pricing and loan configurations |
| `add_on_offer_data` | 4 | Contract modification options |
| `addons_data` | 0 | Add-on transactions (empty in this snapshot) |
| `product_subtypes_data` | 4 | Solar device model catalog |
| `interaction_data` | 2 | Logged guardian-client interactions |
| `issue_data` | 0 | Reported technical issues (empty in this snapshot) |

**Total records across all tables: ~48,000+**

---

## Table Descriptions

### `client_data` — 932 records
Core client profiles for all individuals who have signed a contract with the client.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique client identifier |
| `First Name`, `Family Name` | Client name |
| `Gender`, `Age`, `Birthdate` | Demographic attributes |
| `Longitude`, `Latitude` | GPS coordinates of client location |
| `Primary Phone Number` | Contact number |
| `Has Active/Completed/Defaulted/Late Contracts` | Boolean contract status flags |
| `L0–L4 Entity Id/Name` | Geographic hierarchy (region → zone → headquarters → municipality → village) |
| `User In Charge Id` | The Guardian assigned to this client |
| `Overpaid Amount` | Amount paid beyond what was owed |
| `Start Date`, `End Date` | Period of client activity |

---

### `lead_data` — 945 records
Records for all individuals who were approached as potential customers, whether or not they converted.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique lead identifier |
| `Status` | Outcome of the lead (e.g., `Installed`, `Ready to Buy`, `Not Interested`) |
| `Offer Id`, `Offer Name` | Which solar product was offered |
| `Lead Generator Id/Name` | The agent who sourced this lead |
| `Entry Date`, `Generation Date` | When the lead was logged and when contact was initiated |
| `Decision Date`, `Decision By User Id` | When and by whom the lead outcome was recorded |
| `Client Id`, `Contract Reference` | Populated if the lead converted to a client |
| `L0–L4 Entity Name` | Geographic location of the lead |
| `Reasons For Not Buying` | Captured reason if lead did not convert |

---

### `lead_generator_data` — 21 records
Profiles of the community members who act as sales agents and source new leads.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique lead generator identifier |
| `First Name`, `Family Name` | Agent name |
| `Type` | Role classification (e.g., Sales Leader) |
| `Entity Id`, `Entity Name` | Village or zone the agent is based in |
| `User Id` | Linked Guardian system account |
| `Active` | Whether the agent is currently active |

---

### `user_data` — 25 records
System accounts for all Guardians (field agents) and administrative users who interact with the platform.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique user identifier |
| `First Name`, `Family Name` | User name |
| `Role` | Access level (`Agent`, `Admin`, `SuperAdmin`, `ViewOnly`, `Accountant`, `Metrics API`) |
| `Reference Entity Id/Name` | The village or zone this user is assigned to |
| `Last Web Access`, `Last Mobile Sync` | Activity timestamps |
| `Last Mobile App Version` | Version of the field app in use |

---

### `village_data` — 42 records
Geographic entity table covering all villages and operational zones where the client operates.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique entity identifier |
| `Name` | Village or zone name |
| `Level` | Hierarchy level in the geographic structure |
| `Longitude`, `Latitude` | Coordinates (where available) |
| `Admin Contact Name`, `Admin Phone Number` | Local point of contact |
| `Population` | Village population (sparsely populated in the dataset) |
| `L1–L4 Entity Name` | Parent geographic hierarchy labels |

---

### `repayment_data` — 8,542 records
The primary table used for payment behavior analysis. Each row represents a single scheduled payment event and whether it was made on time.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique repayment record identifier |
| `Contract Id`, `Contract Reference` | Associated contract |
| `Client Id` | Associated client |
| `Payment Date` | Date the payment was actually made |
| `Expected Payment Date` | Date the payment was due |
| `Days Late` | Difference between actual and expected date (negative = paid early) |
| `Total Amount` | Amount due for this payment cycle |
| `Amount Paid` | Amount actually received |
| `Amount Of Discount` | Any discount applied |
| `Credit Value` | Days of solar access credit earned by this payment |
| `Type` | Payment type classification |

*This table was the primary source for engineering features like `avg_days_late`, `max_days_late`, `total_paid`, `total_due`, and `num_payments` used in the default prediction model.*

---

### `payments_data` — 10,898 records
Raw financial transaction log capturing every payment event processed through the system.

| Key Column | Description |
|------------|-------------|
| `Id`, `Transaction Id` | Unique payment and transaction identifiers |
| `Date` | Timestamp of the transaction |
| `Amount` | Payment amount |
| `Source Type` | How payment was made (e.g., `Mentor Cash`, mobile money) |
| `Payment Wallet Name/Id` | Wallet used to receive the payment |
| `Wallet Operator` | Payment operator (e.g., cash, mobile) |
| `Destinations` | Contract(s) this payment was applied to |

---

### `reconciled_payment_data` — 7,012 records
Verified payments that have been matched to specific contract obligations after processing.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique reconciliation record |
| `Date`, `Amount` | When and how much was reconciled |
| `Type` | Type of reconciliation (e.g., `Contract Payment`) |
| `Destination Contract Reference` | Contract the payment was applied to |
| `Origin Payment Id` | Links back to `payments_data` |
| `Client Id`, `Lead Id` | Associated client or lead |
| `Contract Payment Id` | Specific payment obligation fulfilled |

---

### `reversed_payments_data` — 349 records
Payments that were cancelled, refunded, or reversed after initial processing.

| Key Column | Description |
|------------|-------------|
| `Id`, `Reversal Id` | Unique identifiers for the reversal event |
| `Reversal Date` | When the reversal was processed |
| `Status` | Outcome of the reversal (`handled`, etc.) |
| `Approver Id` | Guardian who approved the reversal |
| `Payment Id`, `Payment Transaction ID` | Links back to the original payment |
| `Payment Amount` | Amount reversed |
| `Payment Destinations` | Contract(s) affected |

*Reversal counts per client were used as a risk signal in the default prediction model.*

---

### `contract_event_data` — 3,805 records
A log of all significant events in a contract's lifecycle, from installation through completion, default, or cancellation.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique event record |
| `Contract Id`, `Contract Reference` | Associated contract |
| `Date` | When the event occurred |
| `Type` | Event type (e.g., `Undo Default`, `Repossession`, `Cancellation`, `Completed`) |
| `Approved By User Id`, `Approver User Name` | Guardian who actioned the event |
| `Narration`, `Note` | Free-text context for the event |

*This table was used to identify non-retention events (default, repossession, cancellation, downpayment reversal) for the client retention rate KPI in Objective 3.*

---

### `payment_wallets_data` — 967 records
Wallets held by clients and agents used to receive and process payments.

| Key Column | Description |
|------------|-------------|
| `Id`, `Name` | Wallet identifier and display name |
| `Phone Number` | Associated phone number (for mobile money wallets) |
| `Wallet Operator` | Operator type (e.g., `Cash`, mobile network) |
| `Balance` | Current wallet balance |
| `Total Payments` | Cumulative payments received |
| `Total Reconciliation And Adjustments` | Total amount reconciled out of the wallet |
| `Owner Name` | Client or agent who owns the wallet |
| `First Payment Date` | Date of first transaction |

---

### `stock_data` — 940 records
Inventory of all solar devices, tracking their current location and status.

| Key Column | Description |
|------------|-------------|
| `Id`, `Serial Number` | Device identifier |
| `Type` | Device type code (e.g., `SKG`) |
| `Status` | Current status (`Installed`, `With User`, `In Stock`, etc.) |
| `With Client Id/Name` | Client the device is currently installed with |
| `With User Id/Name` | Guardian currently holding the device |
| `Contract Id` | Contract the device is associated with |
| `Paygo Active Until` | Date through which the client's solar access is paid up |
| `Paygo Mode` | Access model (e.g., `Time`-based credit) |
| `Product Sub Type Id` | Links to the device model catalog |

*Used in Objective 2 to associate device model with connectivity risk.*

---

### `stock_movements_data` — 1,030 records
Tracks every physical movement of a solar device — from warehouse to agent to client and back.

| Key Column | Description |
|------------|-------------|
| `Id`, `Stock Item Id` | Movement and device identifiers |
| `Date` | When the movement occurred |
| `Origin Status`, `Destination Status` | Before and after status of the device |
| `Origin/Destination Client`, `User`, `Entity` | Where the device came from and went to |
| `Status` | Acceptance status of the movement (`accepted`, etc.) |
| `Note`, `Receiving Note` | Free-text notes on the transfer |

---

### `offers_data` — 10 records
The product catalog defining the solar systems available, their pricing, and loan terms.

| Key Column | Description |
|------------|-------------|
| `Id`, `Name`, `Code` | Offer identifier and label |
| `Type` | Financing type (e.g., `Loan`) |
| `Family` | Product family (e.g., `Home`) |
| `Total Value` | Full cost of the system |
| `Deposit Amount` | Required down payment |
| `Loan Duration Days` | Length of the repayment period in days |
| `Reference Payment Frequency Days` | How often payments are expected (e.g., every 30 days) |
| `Minimum Payment` | Minimum accepted payment amount |
| `Reference Pricing Amount` | Standard monthly payment amount |
| `Approval Required` | Whether an admin must approve this offer |

---

### `add_on_offer_data` — 4 records
Available contract modification options that can be applied after a contract is active.

| Key Column | Description |
|------------|-------------|
| `Id`, `Offer Id` | Add-on and linked offer identifiers |
| `Name`, `Code` | Add-on label and code (e.g., `Increase Deposit`) |
| `Category`, `Type` | Classification of the modification |
| `Price Per Unit`, `Deposit Amount` | Cost of the add-on |
| `Available For Leads/Contracts` | Whether it applies at lead or contract stage |
| `Needs Approval` | Whether admin sign-off is required |

---

### `addons_data` — 0 records *(empty in this snapshot)*
Would contain records of add-on modifications actually applied to contracts. No transactions were recorded in the dataset provided for the competition.

---

### `product_subtypes_data` — 4 records
A small lookup table identifying the solar device models in use.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique product subtype identifier |
| `Name` | Device model name (e.g., `Home 400`) |
| `Device Type` | Device category code |
| `Sku` | Stock-keeping unit code |

*Used in Objective 2 as a predictor variable in the disconnection risk model.*

---

### `interaction_data` — 2 records *(sparsely populated)*
Logs of direct interactions between Guardians and clients.

| Key Column | Description |
|------------|-------------|
| `Id` | Unique interaction record |
| `Entry Date`, `Interaction Date` | When the interaction was logged and when it occurred |
| `Interaction Method` | Channel used (e.g., `Phone call`) |
| `Main Topic Name` | Subject of the interaction (e.g., `Support in activation`) |
| `Initiated By Client` | Whether the client or guardian initiated contact |
| `Entry By User Id/Name` | Guardian who logged the interaction |
| `Client Id`, `Client Name` | Client involved |

*Very sparse in this dataset. In a more complete dataset, interaction frequency would be a meaningful signal for both default risk and guardian performance.*

---

### `issue_data` — 0 records *(empty in this snapshot)*
Would contain records of technical issues reported for solar devices. No issues were logged in the dataset provided, which contributed to the challenge of Objective 2 (disconnection risk detection) — the absence of explicit issue records required the team to engineer a proxy label from behavioral contract data instead.

---

## How the Tables Relate

```
village_data
    └── client_data  ←──────────────── user_data (guardians)
            └── lead_data
            └── contract_event_data
            └── repayment_data  ←───── reconciled_payment_data
            └── payments_data   ←───── reversed_payments_data
            └── payment_wallets_data
            └── interaction_data
            └── issue_data
            └── stock_data  ←───────── stock_movements_data
                    └── product_subtypes_data
offers_data ──────────── add_on_offer_data
                                └── addons_data
lead_generator_data ──── lead_data
```

Primary join keys used in our analysis:
- `client_data.Id` ↔ `repayment_data.Client Id`, `lead_data.Client Id`, `interaction_data.Client Id`
- `contract_event_data.Contract Id` ↔ `repayment_data.Contract Id`
- `payments_data.Id` ↔ `reconciled_payment_data.Origin Payment Id`, `reversed_payments_data.Payment Id`
- `stock_data.Product Sub Type Id` ↔ `product_subtypes_data.Id`
- `user_data.Id` ↔ `client_data.User In Charge Id`, `lead_generator_data.User Id`

---

## Data Limitations Noted During Analysis

- **`interaction_data` and `issue_data` were nearly empty**, limiting our ability to use guardian engagement frequency or reported technical issues as model features.
- **Village-level socioeconomic context was absent** — no income levels, infrastructure access, or population data were consistently populated, which constrained geographic risk modeling.
- **Lead conversion metrics may reflect definition ambiguity** — "converted" leads include those marked `Installed` or `Ready to Buy`, but it is unclear whether `Ready to Buy` reliably precedes a signed contract.
- **Contract activity was concentrated in July 2024**, with very few new contracts after that point, limiting time-series analysis across the full dataset.
- **No explicit disconnection label existed**, requiring the team to engineer a proxy for Objective 2 using contract status logic.
