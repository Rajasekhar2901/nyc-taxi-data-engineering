
# NYC Taxi Data Engineering Pipeline

An end-to-end Data Engineering project built using Databricks, PySpark, Delta Lake and Unity Catalog.

The project processes NYC TLC Yellow Taxi trip data using a Medallion Architecture and produces analytics-ready Gold datasets.

---

## Architecture

Raw Data
   ↓
Bronze
   ↓
Silver
   ↓
Gold
   ↓
Business Analytics

### Technologies

- Databricks
- PySpark
- Apache Spark
- Delta Lake
- Unity Catalog
- Python
- SQL
- Parquet

---

## 1. Raw Layer

The pipeline starts with NYC TLC Yellow Taxi trip data in Parquet format.

The original source data is preserved in the Raw layer.

---

## 2. Bronze Layer

The raw Parquet data is converted into Delta format.

Key benefits:

- ACID transactions
- Schema enforcement
- Time travel
- Reliable downstream processing
- Delta Lake storage

---

## 3. Silver Layer

The Silver layer contains cleaned and transformed trip data.

Transformations include:

- Data quality validation
- Invalid trip filtering
- Pickup date extraction
- Pickup year
- Pickup month
- Day of week
- Pickup hour
- Trip duration calculation

Invalid records are separated into a quarantine layer.

---

## 4. Data Quality

The pipeline validates:

- Trip distance
- Fare amount
- Total amount
- Passenger count
- Trip duration

Invalid records are not simply discarded. They are stored separately for investigation.

---

## 5. Taxi Zone Enrichment

NYC Taxi Zone lookup data is joined with trip data to enrich:

- Pickup zone
- Pickup borough
- Drop-off zone
- Drop-off borough

A Broadcast Hash Join is used because the taxi-zone lookup dataset is significantly smaller than the trip dataset.

---

## 6. Gold Layer

The pipeline produces analytics-ready datasets:

### Daily Metrics

- Total trips
- Total revenue
- Average fare
- Average trip distance
- Average trip duration

### Hourly Metrics

- Trips by pickup hour
- Revenue by pickup hour
- Average fare
- Average distance
- Average duration

### Payment Metrics

- Trips by payment type
- Revenue
- Average fare
- Average tip

### Location Metrics

- Trips by pickup location
- Revenue
- Average fare
- Average distance

### Route Metrics

- Pickup zone
- Drop-off zone
- Pickup borough
- Drop-off borough
- Total trips
- Revenue
- Average fare
- Average distance
- Average trip duration

---

## 7. Spark Performance

I inspected the physical execution plan using:

```python
route_metrics.explain("formatted")
