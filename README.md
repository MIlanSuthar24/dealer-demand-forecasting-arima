# 🚛 Commercial Vehicle Dealer Demand Forecasting

<p align="center">
  <img src="https://img.shields.io/badge/Model-ARIMA(1,1,1)-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Language-Python_3-yellow?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/Platform-Google_Colab-orange?style=for-the-badge&logo=googlecolab" />
  <img src="https://img.shields.io/badge/MAPE-12.38%25-brightgreen?style=for-the-badge" />
</p>

> End-to-end time series forecasting pipeline that predicts monthly dealer demand across a commercial vehicle distribution network — enabling smarter inventory planning, leaner supply chains, and proactive production scheduling.

---

## 📌 Table of Contents
- [Business Problem](#-business-problem)
- [Dataset](#-dataset)
- [Project Pipeline](#-project-pipeline)
- [Key Results](#-key-results)
- [Forecast Output](#-forecast-output)
- [Business Recommendations](#-business-recommendations)
- [Repository Structure](#-repository-structure)
- [Author](#-author)

---

## 🔴 Business Problem

Poor demand forecasting in dealer networks causes:

| Problem | Impact |
|---|---|
| Overstocking | High capital lock-up & holding costs |
| Understocking | Lost sales & dealer dissatisfaction |
| Reactive production | Bullwhip effect across the supply chain |
| Weak allocation | Revenue leakage at high-volume dealers |

**Goal:** Predict monthly dealer demand 3 months ahead to support inventory planning, stock allocation, and production scheduling.

---

## 📦 Dataset

**Source:** [Walmart Store Sales Forecasting — Kaggle](https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting), adapted to a commercial vehicle business context.

| Original Column | Business Mapping |
|---|---|
| `Store` | `Dealer_ID` |
| `Dept` | `Vehicle_Segment` |
| `Weekly_Sales` | `Demand_Units` |
| `Type` | `Dealer_Category` |
| `Size` | `Dealer_Capacity` |
| `IsHoliday` | `Seasonal_Indicator` |

**Dataset sizes after loading:**
- `train.csv` → 70,562 rows × 5 cols
- `stores.csv` → 45 rows × 3 cols
- `features.csv` → 8,190 rows × 12 cols

---

## 🔄 Project Pipeline

```
1. Load & Merge          train + stores + features → unified dataset
2. Business Transform    Rename columns to CV domain context
3. Data Cleaning         Interpolate → ffill → bfill → fillna(0) → 0 nulls, 0 duplicates
4. EDA                   Demand trend, dealer category boxplot, seasonal patterns
5. Outlier Treatment     IQR Winsorization → capped at 49,226.87 units
6. Monthly Aggregation   Weekly → Monthly resampling (33 total data points)
7. Stationarity Test     ADF: statistic = -6.73, p = 3.4e-09 → ✅ Stationary
8. Train/Test Split      27 months train (Feb 2010 – Apr 2012) | 6 months test (May – Oct 2012)
9. Model Training        ARIMA(1,1,1) vs Holt-Winters Exponential Smoothing
10. Evaluation           MAE, RMSE, MAPE on held-out test set
11. Final Forecast       Retrain on full data → 3-month outlook
```

---

## 📊 Key Results

### Model Comparison

| Model | MAE | RMSE | MAPE | Winner |
|---|---|---|---|---|
| **ARIMA (1,1,1)** | **3.90M** | **4.07M** | **12.38%** | ✅ |
| Exponential Smoothing | 5.04M | 5.84M | 15.65% | ❌ |

ARIMA outperformed Exponential Smoothing on all metrics — **23% lower MAE** and **3.27 pp lower MAPE** — and was selected as the final forecasting model.

### Stationarity Test (ADF)
- **ADF Statistic:** −6.7253
- **p-value:** 3.40 × 10⁻⁹ *(well below 0.05)*
- **Result:** Series is stationary → ARIMA applicable without additional differencing

### Outlier Treatment
- Upper cap applied at **49,226.87 units** (1.5× IQR)
- Eliminated extreme spikes that would inflate ARIMA parameters and over-forecast future demand

---

## 📅 Forecast Output

ARIMA(1,1,1) retrained on the full 33-month series. **Next quarter forecast:**

| Month | Forecasted Demand | Lower CI (95%) | Upper CI (95%) |
|---|---|---|---|
| Nov 2012 | ~30.43M units | ~20.79M | ~40.59M |
| Dec 2012 | ~30.26M units | ~20.34M | ~40.18M |
| Jan 2013 | ~30.27M units | ~19.95M | ~40.07M |

> ⚠️ The wide confidence interval reflects limited training data (33 months). Safety stock buffers are recommended to account for this range.

---

## 💡 Business Recommendations

| Area | Recommendation |
|---|---|
| **Inventory** | Maintain **15% safety stock** above monthly ARIMA forecast to cover CI variance |
| **Production** | Use **rolling 3-month forecasts** for plant scheduling; refresh monthly |
| **Logistics** | Pre-position stock at least **4 weeks before** seasonal indicator periods |
| **Dealer Strategy** | Prioritize **Category A dealers** for early fulfillment during demand surges |
| **Reporting** | Deploy monthly forecasting dashboards for supply chain visibility |

---

## 📁 Repository Structure

```
dealer-demand-forecasting/
├── dealer_demand_forecasting.ipynb   # Full pipeline: EDA → Model → Forecast
├── dealer_demand_forecasting.py      # Script version
├── demand_forecast_model.pkl         # Serialized model (Exponential Smoothing)
├── train.csv                         # Historical weekly demand (70,562 records)
├── stores.csv                        # Dealer metadata (45 dealers)
├── features.csv                      # Macro & promotional features
└── README.md
```

---

## 👤 Author

**Milan Kumar Suthar** — MSc Statistics & Computing, BHU

*Data Analytics · Machine Learning · Time Series Forecasting · Supply Chain Analytics*

[![GitHub](https://img.shields.io/badge/GitHub-MIlanSuthar24-181717?style=flat&logo=github)](https://github.com/MIlanSuthar24)
