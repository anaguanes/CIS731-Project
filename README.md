# CIS731-Project
## On-Shelf Availability (OSA) Prediction Project  

This project focuses on enhancing **on-shelf availability** (OSA) through predictive analytics, leveraging machine learning techniques to forecast demand fluctuations and identify out-of-stock (OOS) events. The analytics pipeline involves multiple stages, starting from data preparation and proceeding to visualization, model development, and evaluation.

---

## 1. Data Source  
The dataset was obtained from the **Databricks On-Shelf Availability Solution Accelerator**, which provided structured and pre-processed data. The data includes historical sales records, inventory information, replenishment units, and related metrics.  

- **Original Data Structure**:  
  - `date` (daily-level time series)  
  - `store_id` and `sku` (product identifiers)  
  - `total_sales_units`  
  - `on_hand_inventory_units`  
  - `replenishment_units`  
  - `units_in_transit`, `units_in_dc`, and `units_on_order`  

---

## 2. Data Preparation Steps  

### Step 1: Loading Data  
The data was imported into the Databricks environment using Delta Tables to ensure efficient storage and scalable processing.  

```python
inventory_final.repartition(sc.defaultParallelism).write \
    .format('delta') \
    .mode('overwrite') \
    .option('overwriteSchema', 'true') \
    .saveAsTable('osa.inventory')
```

## Step 2: Data Cleaning and Processing  

### Handling Missing Values:  
Missing values in critical columns like `total_sales_units` and `on_hand_inventory_units` were imputed where feasible or dropped if insignificant.  

### Outlier Detection:  
Outliers were addressed using **log transformations** and **IQR-based methods** to ensure robust input for forecasting.  

### Feature Engineering:  
Rolling averages were created to smooth daily sales data. Additional derived metrics included:  
- **Safety Stock**  
- **Lead Time in Transit**  
- **Phantom Inventory Indicators**  

## Step 3: Forecasting Preparation  

The dataset was grouped by `store_id` and `sku` to prepare for time-series forecasting. Missing or inconsistent date ranges were adjusted for each group to ensure proper time series alignment.  

---

## Step 4: Forecasting Techniques  

Two forecasting models were applied:  

- **Simple Exponential Smoothing (SES)**: Baseline model for predicting demand trends.  
- **ARIMA**: Advanced model for comparison and validation of demand predictions.  

## Step 6: Model Evaluation  

The models' performance was evaluated using the following metrics:  

- **Mean Squared Error (MSE)**: 68.70  
- **Accuracy**: 83.16%  
- **Precision**: 89.96%  
- **Recall**: 90.39%  

Additionally, a **paired t-test** was conducted to compare SES and ARIMA forecasts:  

- **T-Statistic**: 0.21  
- **P-Value**: 0.83  
- **Result**: Fail to reject the null hypothesis, indicating no significant difference between SES and ARIMA performance.  

---

## Step 7: Visualizations  

Multiple visualizations were created to analyze and interpret the results, providing actionable insights into sales trends, replenishment needs, and inventory performance:  

1. **Top Categories with Most OOS Alerts**  
2. **Predicted vs Actual Sales Over Time**  
3. **Inventory Status Distribution**  
4. **Units in Transit, DC, and On Order (Log-Transformed)**  
5. **Replenishment Units vs Daily Sales**  
6. **Sales Heatmap by Product Category and Store**  

---

## Pipeline Summary  

The complete pipeline for the **OSA Prediction Project** is summarized below:  

- Data Loading: Import structured inventory and sales data into Delta Tables.
- Data Cleaning: Handle missing values, remove outliers, and engineer key features.
- Forecasting: Use SES and ARIMA models to predict daily sales trends.
- OOS Detection: Identify deviations and flag out-of-stock events.
- Model Evaluation: Validate model performance using MSE, Accuracy, Precision, and Recall.
- Visualization: Generate bar charts, line graphs, scatterplots, and heatmaps to interpret results.
