# Commercial Vehicle Dealer Demand Forecasting using ARIMA

## Project Overview

This project focuses on forecasting future dealer demand for a commercial vehicle distribution network using **Time Series Analytics**. The objective is to build a reliable forecasting solution that helps optimize inventory allocation, reduce stock shortages, and improve supply chain planning.

A public multi-location sales dataset was transformed into a **Commercial Vehicle Dealer Demand Forecasting** business case to simulate real-world dealership operations.

---

## Business Problem Statement

Companies with large dealer networks often face challenges in estimating future demand accurately. Poor forecasting can lead to:

- Excess inventory and high holding costs  
- Stock shortages and lost sales  
- Delays in logistics and dispatch  
- Inefficient production planning  
- Poor dealer allocation decisions  

This project solves the problem by predicting future monthly dealer demand using historical sales trends.

---

## Project Objective

Develop an end-to-end forecasting model to predict future monthly dealer demand and support:

- Inventory Planning  
- Dealer Stock Allocation  
- Supply Chain Optimization  
- Production Scheduling  
- Management Decision-Making  

---

## Dataset Used

**Source:** Walmart Store Sales Forecasting Dataset (Kaggle)

The dataset was restructured into an automotive business use case:

| Original Column | Business Interpretation |
|----------------|-------------------------|
| Store | Dealer_ID |
| Dept | Vehicle_Segment |
| Weekly_Sales | Demand_Units |
| Type | Dealer_Category |
| Size | Dealer_Capacity |
| IsHoliday | Seasonal_Indicator |

### Files Used

- `train.csv`
- `stores.csv`
- `features.csv`

---

## Tools & Technologies

- Python  
- Pandas  
- NumPy  
- Matplotlib  
- Seaborn  
- Statsmodels  
- Scikit-learn  
- Joblib  
- Google Colab  

---

## Project Workflow

1. Data Loading  
2. Data Merging & Transformation  
3. Data Cleaning  
4. Exploratory Data Analysis (EDA)  
5. Outlier Detection & Treatment using IQR  
6. Monthly Demand Aggregation  
7. Stationarity Test (ADF Test)  
8. Train-Test Split  
9. Forecasting Model Development  
10. Model Comparison & Evaluation  
11. Future Demand Forecasting  
12. Business Recommendations  

---

## Exploratory Data Analysis

Key analysis performed:

- Demand trend over time  
- Dealer category demand comparison  
- Seasonal demand pattern detection  
- Demand distribution analysis  

### Key Findings

- Demand remained stable with periodic spikes  
- Category A dealers handled higher demand volumes  
- Seasonal events impacted demand surges  
- Demand data showed right-skewness with outliers  

---

## Outlier Handling

Used **Interquartile Range (IQR)** method with capping (Winsorization) to reduce extreme spikes while preserving time series continuity.

### Why Important?

Outliers can distort forecasting models like ARIMA and reduce accuracy.

---

## Stationarity Test

Performed **Augmented Dickey-Fuller (ADF) Test**

### Result:

- p-value < 0.05  
- Monthly demand series found to be stationary  

This justified use of ARIMA modeling.

---

## Models Used

### 1. ARIMA (Final Selected Model)

Used for capturing trend and time dependency in demand data.

### 2. Exponential Smoothing

Used as benchmark model for comparison.

---

## Model Performance

| Model | MAE (Million) | RMSE (Million) | MAPE |
|------|---------------|----------------|------|
| ARIMA | 19.95 | 21.96 | 11.01% |
| Exp Smoothing | 26.21 | 31.63 | 14.47% |

---

## Final Model Selection

ARIMA outperformed Exponential Smoothing across all evaluation metrics and was selected as the final forecasting model.

---

## Future Demand Forecast (Next 3 Months)

| Month | Forecast Demand (Million) |
|------|----------------------------|
| Month 1 | Predicted |
| Month 2 | Predicted |
| Month 3 | Predicted |

*(Generated dynamically in notebook output)*

---

## Key Business Insights

- Demand is relatively stable with occasional spikes  
- Outlier treatment improved model accuracy significantly  
- ARIMA captured trend better than smoothing methods  
- Category A dealers require priority stock planning  
- Seasonal demand peaks need proactive allocation  

---

## Business Recommendations

### Inventory Planning

Maintain safety stock above forecast demand.

### Production Planning

Use rolling 3-month forecasts for plant scheduling.

### Logistics Optimization

Allocate transport capacity based on predicted regional demand.

### Dealer Strategy

Prioritize high-volume dealers for faster replenishment.

### Reporting

Deploy monthly forecasting dashboards for management review.

---

## Final Conclusion

This project successfully developed a complete **Commercial Vehicle Dealer Demand Forecasting pipeline** using Python and ARIMA. By combining data transformation, outlier treatment, stationarity testing, and forecasting analytics, the solution supports smarter inventory planning, lower costs, and improved supply chain efficiency.

---

## Files in Repository

- `dealer_demand_forecasting.ipynb`
- `dealer_demand_forecasting.py`
- `train.csv`
- `stores.csv`
- `features.csv`
- `demand_forecast_model.pkl`

---

## Author

**Milan Kumar Suthar**

- MSc Statistics & Computing, BHU  
- Data Analytics | Machine Learning | Forecasting | Supply Chain Analytics

---
