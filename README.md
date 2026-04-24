# Commercial Vehicle Dealer Demand Forecasting

## Project Overview

This project focuses on forecasting future dealer demand for a commercial vehicle distribution network using time series analytics. The objective was to build a reliable forecasting solution that helps optimize inventory allocation, reduce stock shortages, and improve supply chain planning.

A public multi-location sales dataset was adapted into a dealer demand forecasting business case to simulate real-world dealership planning scenarios.

---

## Business Problem Statement

Companies with large dealer networks often struggle to estimate future demand accurately. Poor forecasting can lead to:

* Excess inventory and high holding costs
* Stock shortages and lost sales
* Delays in logistics and dispatch
* Inefficient production planning
* Poor dealer allocation decisions

This project solves the problem by predicting future monthly dealer demand using historical demand trends.

---

## Project Objective

Develop an end-to-end forecasting model to predict monthly dealer demand and support:

* Inventory planning
* Dealer stock allocation
* Supply chain optimization
* Management decision-making

---

## Dataset Used

**Source:** Walmart Store Sales Forecasting Dataset (Kaggle)

The dataset was restructured for this business case as follows:

| Original Column | Business Interpretation |
| --------------- | ----------------------- |
| Store           | Dealer_ID               |
| Dept            | Vehicle Segment         |
| Weekly_Sales    | Demand Units            |
| Type            | Dealer Category         |
| Size            | Dealer Capacity         |
| IsHoliday       | Seasonal Indicator      |

Files Used:

* train.csv
* stores.csv
* features.csv

---

## Tools & Technologies

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Statsmodels
* Scikit-learn
* Google Colab

---

## Project Workflow

1. Data loading and merging
2. Data cleaning and preprocessing
3. Exploratory Data Analysis (EDA)
4. Monthly demand aggregation
5. Forecasting model development
6. Model comparison and evaluation
7. Future demand forecasting
8. Business recommendations

---

## Models Used

* Exponential Smoothing
* ARIMA (Final Selected Model)

---

## Evaluation Metrics

* MAE (Mean Absolute Error)
* RMSE (Root Mean Squared Error)
* MAPE (Mean Absolute Percentage Error)

### Model Performance

| Model                 | MAE (Million) | RMSE (Million) |   MAPE |
| --------------------- | ------------: | -------------: | -----: |
| ARIMA                 |         24.44 |          25.40 | 11.86% |
| Exponential Smoothing |         30.70 |          36.32 | 14.55% |

### Final Model Selection

ARIMA outperformed Exponential Smoothing and was selected as the final forecasting model.

---

## Forecast Output

Next 3 Months Predicted Demand:

| Month   | Forecast Demand (Million) |
| ------- | ------------------------: |
| 2012-11 |                    200.40 |
| 2012-12 |                    200.14 |
| 2013-01 |                    200.15 |

---

## Key Insights

* Dealer demand remained stable in the short-term forecast horizon.
* ARIMA produced more accurate forecasts than Exponential Smoothing.
* Forecasting can reduce stock shortages and excess inventory.
* Seasonal demand spikes require proactive stock planning.
* Forecast dashboards can improve management decisions.

---

## Business Recommendations

### Inventory Planning

Increase stock allocation for high-demand dealers before seasonal peaks.

### Supply Chain Optimization

Use rolling forecasts for transport and warehouse planning.

### Dealer Strategy

Prioritize stock movement toward top-performing dealers.

### Decision Support

Use monthly forecasting dashboards for proactive planning.

---

## Final Conclusion

This project successfully developed an end-to-end dealer demand forecasting pipeline using ARIMA and time series analytics. The solution can support dealer allocation, inventory planning, and supply chain efficiency in commercial vehicle businesses.


---

## Author

Milan Kumar Suthar

---
