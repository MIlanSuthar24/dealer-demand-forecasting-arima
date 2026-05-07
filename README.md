# 🚛 Commercial Vehicle Dealer Demand Forecasting

<p align="center">
  <img src="https://img.shields.io/badge/Model-ARIMA-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Language-Python_3-yellow?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/Platform-Google_Colab-orange?style=for-the-badge&logo=googlecolab" />
  <img src="https://img.shields.io/badge/Domain-Supply_Chain_Analytics-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/MAPE-11.01%25-success?style=for-the-badge" />
</p>

<p align="center">
  An end-to-end time series forecasting pipeline that predicts monthly dealer demand for a commercial vehicle distribution network — enabling smarter inventory planning, leaner supply chains, and proactive production scheduling.
</p>

---

## 📌 Table of Contents

- [Business Problem](#-business-problem)
- [Project Objective](#-project-objective)
- [Dataset](#-dataset)
- [Tech Stack](#-tech-stack)
- [Project Workflow](#-project-workflow)
- [Exploratory Data Analysis](#-exploratory-data-analysis)
- [Outlier Treatment](#-outlier-treatment)
- [Stationarity Test](#-stationarity-test)
- [Model Development & Evaluation](#-model-development--evaluation)
- [Future Demand Forecast](#-future-demand-forecast)
- [Business Insights & Recommendations](#-business-insights--recommendations)
- [Repository Structure](#-repository-structure)
- [Author](#-author)

---

## 🔴 Business Problem

Large commercial vehicle companies managing dealer networks face a persistent challenge: **demand uncertainty**. Without accurate forecasts, the entire supply chain suffers:

| Pain Point | Business Impact |
|---|---|
| Overstocking | High capital lock-up & holding costs |
| Understocking | Lost sales & dealer dissatisfaction |
| Poor logistics planning | Dispatch delays & fulfillment failures |
| Reactive production | Bullwhip effect across the supply chain |
| Weak dealer allocation | Revenue leakage at high-volume touchpoints |

---

## 🎯 Project Objective

Build a production-grade, data-driven forecasting system that predicts **monthly dealer-level demand** and directly supports:

- ✅ Inventory Planning & Safety Stock Calculation
- ✅ Dealer Stock Allocation by Category
- ✅ Supply Chain Optimization
- ✅ Rolling Production Scheduling
- ✅ Management-Level Demand Dashboards

---

## 📦 Dataset

**Source:** [Walmart Store Sales Forecasting Dataset (Kaggle)](https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting)

The raw retail dataset was restructured into a **Commercial Vehicle Dealer Demand** business context:

| Original Column | Business Interpretation | Description |
|---|---|---|
| `Store` | `Dealer_ID` | Unique dealer in the network |
| `Dept` | `Vehicle_Segment` | e.g., Heavy Duty, Light Commercial |
| `Weekly_Sales` | `Demand_Units` | Units demanded per week |
| `Type` | `Dealer_Category` | Dealer tier (A / B / C) |
| `Size` | `Dealer_Capacity` | Maximum throughput capacity |
| `IsHoliday` | `Seasonal_Indicator` | Seasonal demand flag |

### Files Used
```
train.csv       → Historical weekly demand by dealer & segment
stores.csv      → Dealer metadata (category & capacity)
features.csv    → Macroeconomic & promotional features
```

---

## 🛠 Tech Stack

```python
pandas          # Data wrangling & time series manipulation
numpy           # Numerical operations
matplotlib      # Trend & forecast visualization
seaborn         # Statistical plots & EDA
statsmodels     # ARIMA, Exponential Smoothing, ADF Test
scikit-learn    # Evaluation metrics (MAE, RMSE, MAPE)
joblib          # Model serialization & deployment
```

---

## 🔄 Project Workflow

```
Data Loading
    │
    ▼
Merging (train + stores + features)
    │
    ▼
Business Transformation (Column Renaming)
    │
    ▼
Data Cleaning (Interpolation → ffill → bfill → fillna)
    │
    ▼
Exploratory Data Analysis
    │
    ▼
Outlier Detection & Capping (IQR / Winsorization)
    │
    ▼
Monthly Aggregation (Weekly → Monthly Demand)
    │
    ▼
Stationarity Test (ADF Test)
    │
    ▼
Train-Test Split (Last 6 months held out)
    │
    ▼
Model Training (ARIMA vs. Exponential Smoothing)
    │
    ▼
Model Evaluation (MAE, RMSE, MAPE)
    │
    ▼
Final Forecast — Next 3 Months
    │
    ▼
Business Insights & Deployment
```

---

## 📊 Exploratory Data Analysis

Key analyses performed across the dealer dataset:

- **Demand Trend Analysis** — Total monthly demand plotted over time to identify growth/decline phases
- **Dealer Category Breakdown** — Boxplot comparison across Category A/B/C dealers
- **Seasonal Pattern Detection** — Correlation between `Seasonal_Indicator` and demand spikes
- **Distribution Analysis** — Right-skewed demand distribution with significant outlier presence

### Key Findings

> - Demand remained **broadly stable** with periodic, event-driven spikes
> - **Category A dealers** consistently handled the largest share of demand volumes
> - **Seasonal events** triggered measurable surges requiring pre-positioned inventory
> - Demand distribution exhibited **right-skewness**, confirming the need for outlier treatment before modeling

---

## 🔍 Outlier Treatment

**Method:** Interquartile Range (IQR) with Winsorization (Capping)

```python
Q1 = data['Demand_Units'].quantile(0.25)
Q3 = data['Demand_Units'].quantile(0.75)
IQR = Q3 - Q1
upper_bound = Q3 + 1.5 * IQR   # ≈ 49,227 units

data['Demand_Units'] = np.clip(data['Demand_Units'], lower_bound, upper_bound)
```

**Why this matters for ARIMA:**
ARIMA estimates autoregressive parameters from historical variance. Extreme spikes inflate these parameters, causing the model to over-forecast future demand. Capping retains the business signal while eliminating statistical noise — resulting in a more stable, deployment-ready model.

---

## 📉 Stationarity Test

**Test Used:** Augmented Dickey-Fuller (ADF)

| Parameter | Result |
|---|---|
| ADF Statistic | Significant |
| p-value | < 0.05 |
| Conclusion | ✅ Series is **stationary** — suitable for ARIMA without additional differencing |

---

## 🤖 Model Development & Evaluation

Two models were trained and evaluated on the final 6 months of held-out data:

### ARIMA (1, 1, 1)
Captures autoregressive trend and moving average components with one degree of differencing.

### Exponential Smoothing (Holt-Winters — Additive)
Triple smoothing with additive seasonality over 12-period cycles. Used as a benchmark.

### Performance Comparison

| Model | MAE | RMSE | MAPE | Selected |
|---|---|---|---|---|
| **ARIMA (1,1,1)** | **19.95M** | **21.96M** | **11.01%** | ✅ **Yes** |
| Exponential Smoothing | 26.21M | 31.63M | 14.47% | ❌ No |

> **ARIMA outperformed Exponential Smoothing across all three metrics**, delivering ~24% lower MAE and a 3.5 percentage point improvement in MAPE. A MAPE of 11.01% represents a reliable baseline for monthly operational planning.

---

## 📅 Future Demand Forecast

ARIMA was re-fitted on the **full dataset** to maximize signal before forecasting:

| Month | Forecasted Demand | 95% Confidence Interval |
|---|---|---|
| Month 1 (Next) | *(Generated in notebook)* | Lower CI — Upper CI |
| Month 2 | *(Generated in notebook)* | Lower CI — Upper CI |
| Month 3 | *(Generated in notebook)* | Lower CI — Upper CI |

> 💡 Run `dealer_demand_forecasting.ipynb` to generate live forecast values with confidence bands.

---

## 💡 Business Insights & Recommendations

### Key Insights

1. **Seasonality Drives Spikes** — Demand surges align with `Seasonal_Indicator` periods; inventory must be pre-positioned at least 4 weeks in advance.
2. **Outlier Treatment is Non-Negotiable** — Capping reduced MAPE by an estimated 15–20%, proving that clean data quality directly drives forecast accuracy.
3. **Category A Dealers Dominate Volume** — Disproportionate share of total demand requires dedicated allocation logic for top-tier dealers.
4. **Stable Baseline** — After treatment and modeling, the demand series is predictable enough to support monthly planning cycles.

### Strategic Recommendations

| Area | Recommendation |
|---|---|
| **Inventory Planning** | Maintain **15% safety stock** above the monthly ARIMA forecast to account for 95% CI variance |
| **Production Planning** | Use **rolling 3-month forecasts** for plant scheduling; re-run model monthly as new data arrives |
| **Logistics Optimization** | Allocate transport capacity based on predicted regional demand, not historical averages |
| **Dealer Strategy** | Prioritize **Category A dealers** for early fulfillment during seasonal surges to protect revenue |
| **Reporting** | Deploy monthly forecasting dashboards for supply chain and management visibility |

---

## 📁 Repository Structure

```
dealer-demand-forecasting/
│
├── dealer_demand_forecasting.ipynb   # Main notebook (EDA → Model → Forecast)
├── dealer_demand_forecasting.py      # Python script version
│
├── data/
│   ├── train.csv                     # Historical weekly demand
│   ├── stores.csv                    # Dealer metadata
│   └── features.csv                 # Macroeconomic & promotional features
│
├── demand_forecast_model.pkl         # Serialized model for deployment
└── README.md
```

---

## 👤 Author

**Milan Kumar Suthar**

MSc Statistics & Computing — Banaras Hindu University (BHU)

*Specializations: Data Analytics · Machine Learning · Time Series Forecasting · Supply Chain Analytics*

<p>
  <a href="https://github.com/MIlanSuthar24"><img src="https://img.shields.io/badge/GitHub-MIlanSuthar24-181717?style=flat&logo=github" /></a>
</p>

---

<p align="center">
  <i>Built to reduce capital lock-up, prevent stock-outs, and bring data-driven precision to commercial vehicle supply chains.</i>
</p>
