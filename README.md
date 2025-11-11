Material Master Delta Pipeline 
This project implements a simple Lakehouse Bronze → Silver Delta pipeline for daily material master data using Databricks Community Edition.
## Bronze Layer
Source file: pipe-delimited (|) CSV uploaded to Databricks.
Stored as a managed Delta table:
healthcare.default.material_master_1_k
No transformations applied (raw ingestion).
## Silver Layer
Cleaned and standardized Delta table created as:
healthcare.default.silver_material_master
Transformations applied:
Trimmed all string columns
Converted blank values ("", null, NA, N/A) → NULL
Casted unit_cost to DOUBLE
Parsed last_updated into DATE
Dropped duplicate rows
Expectation implemented:
material_id must NOT be NULL
(Additional checks applied: unit_cost >= 0, valid status)
## Files Included
Material_Master_Pipeline.html — exported Databricks notebook with outputs
Material_Master_Pipeline.py — source code
/screenshots — identity proof, Bronze/Silver counts, samples, table details
### How to Run
USE CATALOG healthcare;
USE SCHEMA default;
SELECT COUNT(*) FROM material_master_1_k;
SELECT COUNT(*) FROM silver_material_master;
SELECT * FROM silver_material_master LIMIT 20;
DESCRIBE DETAIL silver_material_master; Notes
Notes- 
Databricks
 does not support public DBFS, so both layers use managed Delta tables.
Daily CSV arrivals can be loaded into Bronze and the notebook can be rerun to refresh Silver.
