# Regulatory Policy Simulation & Dashboard

An end-to-end econometric simulation and executive analytics dashboard evaluating the microeconomic impact of a proposed **25% profit margin cap** across 160 transaction records. 

This project combines a **relational SQL database** for data storage and extraction, a **Python simulation** modelling consumer demand responses under price elasticity constraints, and an interactive **Power BI dashboard** to aid with executive decision-making.

---

## Executive Summary & Core Insights

* **Top-Line Revenue Neutrality (0.0% Change):** Because consumer demand was assumed to be inelastic and constant ($\text{PED} = -0.75$), lower unit prices driven by the regulatory cap stimulated increased transaction volume. This volume expansion offset unit price cuts, leaving gross revenue virtually unchanged.
* **Bottom-Line Margin Squeeze (-9.8% Net Profit Drop):** Fixed unit costs combined with capped profit margins resulted in a severe net profit reduction across the portfolio, dropping overall profit from **£168K** to **£152K**.
* **Asymmetric Firm Exposure:** The policy disproportionately penalises market leaders and efficient operators. High-margin firms (e.g. **Firm E** at **-11.3%**) experienced severe profit drops, whereas lower-margin competitors (e.g. **Firm A** at **-2.3%**) were affected much less.

---

## Methodology

### 1. Data Ingestion & SQL Querying (`SQL / SQLite')
* Stored raw transaction logs across firm, product, regional, and temporal dimensions in a relational **SQLite** database.
* Executed SQL scripts to filter, join, and extract clean relational datasets into Python for econometric modelling.

### 2. Microeconomic Elasticity Simulation (`Python`)
* **Price Elasticity Calibration:** Calibrated using an assumed inelastic price elasticity of demand:
  $$\text{PED} = \frac{\% \Delta Q}{\% \Delta P} = -0.75$$
* **Policy Margin Capping:** Evaluated transaction-level margins. For products exceeding the $25\%$ profit margin limit:
  $$\text{New Price} = \text{Unit Cost} \times 1.25$$
* **Volume Adjustment:** Unit sales volumes were dynamically adjusted using Pandas and NumPy to model true market behavioural responses post-regulation.
* **Visualisation:** Seaborn and Matplotlib were utilised to analyse price distributions at baseline vs post-policy, profit impact, and the distribution of profit losses

### 3. Interactive Executive Reporting (`Power BI & DAX`)
* Built dynamic DAX measures to isolate revenue/profit variances across firms, regions, and quarters.
* Structured a clean UI featuring KPI cards, firm-level loss comparisons, regional vulnerability matrices, and quarterly trajectory charts.

---

## Key DAX Measures

```dax
// Profit Variance Percentage
Profit Change % = 
DIVIDE(
    [Total Policy Profit] - [Total Baseline Profit],
    [Total Baseline Profit],
    0
)

// Capped Transaction Counter
Affected Transactions = 
CALCULATE(
    COUNTROWS(simulated_transactions),
    simulated_transactions[pct_change_price] < 0
)
```
---

## Repository Structure

```text
regulatory-policy-simulation/
├── data/
│   ├── transactions.db
│   └── raw_transactions.csv
├── sql/
│   └── schema_and_queries.sql
├── notebooks/
│   └── policy_simulation_engine.ipynb
├── outputs/
│   └── simulated_transactions.csv
├── dashboard/
│   ├── Regulatory_Policy_Simulation.pbix
│   └── dashboard_preview.png
└── README.md

```
---

## Tools & Tech Stack

* **Database & Querying:** SQL / SQLite
* **Language:** Python 3.x
* **Libraries:** Pandas, NumPy, Matplotlib, Seaborn, `sqlite3`
* **BI Platform:** Microsoft Power BI Desktop
* **Language/Calculations:** Data Analysis Expressions (DAX)
