# End‑to‑End Brain Tumor Detection Pipeline on Azure

we create an End-to-end Azure pipeline for brain tumor MRI classification using ADF, Databricks, data cleaning, feature engineering, threshold‑based model training, deployment, and monitoring.

This project implements a complete end‑to‑end data pipeline on Azure to classify brain MRI images as tumor or normal using a metadata‑based baseline model.
The goal is to demonstrate the full data‑science lifecycle using Azure services such as Azure Data Lake Storage, Azure Data Factory, and Azure Databricks.

# Project Overview
We used a brain MRI dataset containing image files labeled as “yes” (tumor) or “no” (normal).
Instead of working directly with image pixels (due to cluster restrictions), we built a full pipeline using image metadata, focusing on data engineering, preprocessing, and creating a simple baseline classifier.

# Pipeline Steps
1. Data Ingestion:
   
-Uploaded dataset to Azure Data Lake Storage (ADLS) under a structured Lakehouse folder.

-Built an ADF Copy Data pipeline to convert metadata from CSV → Parquet.

2. Data Cleaning (Databricks):
   
-Removed corrupted or missing image entries.

-Dropped images where file size ≤ 0.

-Created numeric label (label_index: 1 = tumor, 0 = normal).

-Saved cleaned metadata to the Gold layer.

3. Feature Engineering:
   
-Created engineered features from metadata:

size_kb (image size in kilobytes)

size_bucket (small / medium / large)

Split dataset into train (70%), validation (15%), and test (15%).

Saved engineered datasets as Delta tables.

4. Baseline Model (Threshold Classifier):
Because Spark ML algorithms were restricted, we trained a threshold‑based model:

Predict "tumor" if size_kb > threshold.

We tested thresholds: 80, 40, 20, 15, 10, 5

and selected 10 KB as the best-performing threshold.

** Final Model Performance (Threshold = 10 KB)** 
Accuracy: ~73%
Precision: ~70%
Recall: ~94% (most important for medical detection)

5. Deployment & Monitoring:
   
-Deployed the model as a reusable Spark UDF (predict_tumor()).

-Evaluated the model on test data.

-No drift observed between validation and test performance.

# Summary:
This project demonstrates a complete Azure ML pipeline including:

Cloud data ingestion

ETL + cleaning

Feature engineering

Model training & tuning

Deployment

Monitoring

The pipeline is fully functional and provides a strong foundation for future improvements such as using CNN models on the actual MRI images.


