# Regulatory Policy Simulation & Executive Impact Dashboard

An end-to-end econometric simulation and executive analytics dashboard evaluating the microeconomic impact of a proposed **25% profit margin cap** across 160 transaction records. 

This project combines a **Python simulation** modelling consumer demand responses under price elasticity constraints with a **Power BI dashboard** to display a clean dashboard for executive decision-making.

---

## Executive Summary & Core Insights

* **Top-Line Revenue Neutrality ($0.0\%$ Change):** Because consumer demand was calibrated as inelastic ($\text{PED} = -0.75$), lower unit prices driven by the regulatory cap stimulated increased transaction volume. This volume increase offset unit price cuts, leaving overall revenue virtually unchanged.
* **Bottom-Line Margin Squeeze ($-9.8\%$ Net Profit Drop):** Fixed unit costs combined with capped profit margins resulted in a severe net profit reduction across the portfolio, dropping overall profit from **£168K** to **£152K**.
* **Asymmetric Firm Exposure:** The policy disproportionately penalises market leaders and efficient operators. High-margin firms (e.g **Firm E** at **$-11.3\%$**) experienced severe profit drops, whereas lower-margin competitors (e.g **Firm A** at **$-2.3\%$**) were unaffected much less.

---

## Technical Architecture & Methodology
### 1. Microeconomic Elasticity Simulation ('Python')
* **Price Elasticity Calibration:** Calibrated using an assumed inelastic price elasticity of demand:
  $$\text{PED} = \frac{\% \Delta Q}{\% \Delta P} = -0.75$$
* **Policy Margin Capping:** Evaluated transaction-level margins. For products exceeding the $25\%$ profit margin limit:
  $$\text{New Price} = \text{Unit Cost} \times 1.25$$
* **Volume Adjustment:** Unit sales volumes were dynamically adjusted upstream using Pandas and NumPy to model true market behavioural responses post-regulation.

### 2. Interactive Executive Reporting (`Power BI & DAX`)
* Built dynamic DAX measures to isolate revenue/profit variances across firms, regions, and quarters.
* Structured a UI featuring KPI cards, firm-level loss comparisons, regional vulnerability matrices, and quarterly trajectory charts.

---

## Key DAX Measures

```dax
// Profit Change
Profit Change % = 
DIVIDE(
    [Total Policy Profit] - [Total Baseline Profit],
    [Total Baseline Profit],
    0
)

// Number of Affected Transactions
Affected Transactions = 
CALCULATE(
    COUNTROWS(simulated_transactions),
    simulated_transactions[pct_change_price] < 0
)
