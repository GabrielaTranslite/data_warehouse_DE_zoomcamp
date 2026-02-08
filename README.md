# data_warehouse_DE_zoomcamp
Module 3 of Data Engineering Zoomcamp
# data_warehouse_DE_zoomcamp
Module 3 of Data Engineering Zoomcamp

# Creating an external table referring to gcs path
CREATE OR REPLACE EXTERNAL TABLE `module_3.yellow_taxi_homework`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://dezoomcamp_hw3_2026_gabriela/yellow_tripdata_2024-01.parquet',
  'gs://dezoomcamp_hw3_2026_gabriela/yellow_tripdata_2024-02.parquet',
  'gs://dezoomcamp_hw3_2026_gabriela/yellow_tripdata_2024-03.parquet',
  'gs://dezoomcamp_hw3_2026_gabriela/yellow_tripdata_2024-04.parquet',
  'gs://dezoomcamp_hw3_2026_gabriela/yellow_tripdata_2024-05.parquet',
  'gs://dezoomcamp_hw3_2026_gabriela/yellow_tripdata_2024-06.parquet']
);

# Question 1
SELECT COUNT(*) FROM `kestra-sandbox-486017.module_3.yellow_taxi_homework`
20332093

# Question 2
SELECT COUNT(DISTINCT PULocationID) FROM `kestra-sandbox-486017.module_3.yellow_taxi_homework_non_partitioned` 155.12 MB
SELECT COUNT(DISTINCT PULocationID) FROM `kestra-sandbox-486017.module_3.yellow_taxi_homework`
0 MB

# Question 3
BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.

# Question 4
SELECT COUNT(fare_amount) FROM `kestra-sandbox-486017.module_3.yellow_taxi_homework`
WHERE fare_amount = 0
8,333

# Question 5
SELECT DISTINCT VendorID FROM `kestra-sandbox-486017.module_3.yellow_taxi_homework_partitioned_clustered` 
WHERE TIMESTAMP_TRUNC(tpep_dropoff_datetime, DAY) >= TIMESTAMP("2024-03-01") AND TIMESTAMP_TRUNC(tpep_dropoff_datetime, DAY) <= TIMESTAMP("2024-03-15")
26.84 MB

SELECT DISTINCT VendorID FROM `kestra-sandbox-486017.module_3.yellow_taxi_homework_non_partitioned` 
WHERE TIMESTAMP_TRUNC(tpep_dropoff_datetime, DAY) >= TIMESTAMP("2024-03-01") AND TIMESTAMP_TRUNC(tpep_dropoff_datetime, DAY) <= TIMESTAMP("2024-03-15")
310.24 MB
