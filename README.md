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
