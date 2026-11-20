# Inventory Optimization & ABC-XYZ Demand Analytics

An end-to-end Supply Chain Analytics project built in Python to segment a multi-thousand SKU retail catalog, model stochastic safety stocks under joint demand and supplier lead-time uncertainty, and quantify the working capital trade-off across service levels.

## 1. Business Problem
Applying uniform replenishment policies across a heterogeneous catalog creates two simultaneous operational failures:
* **Stockouts on high-value items**, causing direct revenue loss and customer dissatisfaction.
* **Excess Working Capital tied up in slow-moving, erratic items**, increasing holding costs and obsolescence risk.

## 2. Methodology & Data Pipeline
Using the **Online Retail II** transactional dataset (UCI / Kaggle, 500k+ records):
1. **Data Cleaning:** Removed cancelled invoices (`C`), negative returns, and non-physical service codes (`POST`, `BANK CHARGES`, etc.) to isolate clean primary demand.
2. **Weekly Time-Bucketing:** Resampled intraday transactions into fixed weekly planning buckets, explicitly filling zero-sales weeks to avoid distorting demand variance.
3. **ABC-XYZ Segmentation:** Combined Pareto revenue contribution (80/15/5) with demand predictability measured via Coefficient of Variation ($CV = \sigma_d / \mu_d$).
4. **Stochastic Safety Stock Modeling:** Computed buffer requirements under combined demand and supplier lead-time uncertainty ($LT = 2.0$ weeks, $\sigma_{LT} = 0.5$ weeks):
   $$SS = Z \cdot \sqrt{LT \cdot \sigma_d^2 + \mu_d^2 \cdot \sigma_{LT}^2}$$

## 3. Key Findings & Executive Visuals

### Portfolio Distribution (ABC-XYZ Matrix)
![ABC-XYZ Matrix](abc_xyz_heatmap.png)

### Working Capital vs. Service Level Trade-Off
![Working Capital Trade-Off](working_capital_tradeoff.png)

* **Non-linear capital impact:** Upgrading the target service level from **95% to 99%** requires a **+41.4% surge in tied-up working capital** to capture just 4% of incremental demand coverage.

## 4. Recommended Differentiated Inventory Policies

| Segment | Profile | Operational Strategy |
| :--- | :--- | :--- |
| **AX / BX** | High value, stable demand | **Continuous Review / JIT:** Tight safety stocks, frequent automated replenishment, strong supplier SLAs. |
| **AY / BY** | High value, seasonal/variable | **Collaborative S&OP:** Periodic forecast review with safety stocks calibrated at a 95% service level. |
| **AZ** | High value, erratic demand | **Make-to-Order / Centralized Stock:** Avoid local buffering; trigger procurement upon confirmed customer orders. |
| **CX / CY** | Low value, predictable | **Two-Bin / Min-Max:** Automated replenishment with larger order quantities to minimize administrative costs. |
| **CZ** | Low value, highly erratic | **Catalog Rationalization / De-stocking:** Eliminate dead stock or manage strictly on a spot-order basis. |

## 5. Tech Stack
* **Python:** Pandas, NumPy, SciPy (statistical distributions), Matplotlib & Seaborn
* **Environment:** Google Colab / Jupyter Notebook
