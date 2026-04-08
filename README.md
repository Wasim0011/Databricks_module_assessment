# HR Payroll & Talent Analytics Pipeline 🚀
### Databricks Medallion Architecture Implementation

This repository contains a full-scale Data Engineering pipeline built on **Databricks**, leveraging **Delta Lake** and **Unity Catalog**. The project transforms raw HR data into actionable business intelligence through a three-layer Medallion architecture, ensuring data integrity, historical tracking, and high-performance querying.

---

## 🏗️ Architecture Overview

The pipeline follows the industry-standard **Medallion Architecture**:

* **Bronze (Raw):** Landing zone for source CSVs. Data is ingested with audit metadata (`_source_file`, `_ingested_at`) using **Auto Loader** for incremental updates.
* **Silver (Enriched):** Data is cleaned, joined, and normalized. 
    * **Logic:** Tenure calculations, salary normalization (Window Functions), and **SCD Type 2** implementation for tracking historical job title changes.
* **Gold (Curated):** Business-ready views optimized for executive dashboards, focusing on monthly budgets and senior talent distribution.

---

## 🛠️ Technical Stack & Features

* **Compute:** Databricks SQL & Spark (PySpark).
* **Storage:** Delta Lake (with ACID transactions).
* **Governance:** Unity Catalog (3-level namespace: `catalog.schema.table`).
* **Ingestion:** Auto Loader (`cloudFiles`) with schema evolution.
* **Optimization:** Z-Ordering, VACUUM, and ANALYZE for storage efficiency.
* **Data Quality:** Delta CHECK Constraints for schema-level validation.
* **CDC:** Change Data Feed (CDF) enabled for downstream payroll auditability.

---

## 📋 Task Breakdown

### Phase 1: Ingestion & Quality (Bronze)
* Staged HR datasets (Employees, Salaries, Departments) in DBFS.
* Applied **CHECK Constraints** to enforce valid employment types.
* Captured ingestion metadata using Unity Catalog-compatible `_metadata` hidden columns.

### Phase 2: Business Logic & History (Silver)
* **Tenure Logic:** Computed `tenure_years` dynamically based on hire date.
* **Salary Normalization:** Used `ROW_NUMBER()` over partition windows to ensure only the latest salary records are active.
* **SCD Type 2:** Developed a `MERGE` strategy to track career progression (job title changes) without losing historical data.

### Phase 3: Reporting & Optimization (Gold)
* Created aggregation views for **Monthly Department Budgets**.
* Built a **Seniority Report** for employees with 5+ years of tenure.
* Implemented **Z-Ordering** on high-cardinality columns to reduce I/O overhead.

---

## 🚀 How to Run

1.  **Clone** this repository into your Databricks Workspace via **Repos**.
2.  Ensure you are on the `wasim-changes` branch.
3.  Execute the `hr_payroll_pipeline` notebook.
    * *Note: Ensure your user has `CREATE` permissions on the `filestore` catalog.*
4.  View the final outputs in the `filestore.gold` schema.

---

## 📊 Business Insights
The final Gold layer powers high-level reporting that provides:
* **Budget Clarity:** Real-time visibility into departmental spend including bonuses.
* **Retention Tracking:** Identification of long-term talent for reward programs.
* **Historical Audits:** Full career-path tracking via the SCD Type 2 table.

---

**Author:** Md Wasim  
**Date:** April 2026  
**Project:** Databricks Module Assessment
