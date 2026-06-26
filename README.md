# 💡 Intelligent Financial Operations Analytics Platform

> End-to-end Data Engineering pipeline built entirely on **Microsoft Fabric** —
> from raw source data to business-ready Power BI dashboards.

## 📌 Overview

A telecom company's operational data lives in three completely separate systems
with no integration, no data quality enforcement, and no unified view for
business decision-making.

This platform solves that by building a fully automated, incremental data
pipeline that:

- Ingests data from **MySQL**, **PostgreSQL**, and **Google Sheets**
- Validates and cleans data through a **high DQ framework**
- Models it into a **star schema** in Fabric Warehouse

## 🏗️ Architecture
│                        DATA SOURCES                             │

│                                                                 │

│   MySQL (Aiven)    PostgreSQL (Supabase)    Google Sheets       │

│   • customers      • towers                • support_tickets    │

│   • subscriptions  • network_events                             │

│   • fx_rates       • vendors                                    │

└────────────────────────┬────────────────────────────────────────┘

│  Incremental Watermark Load

▼

┌─────────────────────────────────────────────────────────────────┐

│                  BRONZE LAYER (Raw Parquet)                     │

│              Files/bronze/  — 7 tables — No transformation      │

└────────────────────────┬────────────────────────────────────────┘

│  PySpark DQ Transformation

▼

┌─────────────────────────────────────────────────────────────────┐

│             SILVER LAYER (Delta Lake)                           │

│   Files/silver/  — 7 tables — Cleaned · Typed · DQ Validated   │

│   Delta MERGE upsert · DQ logs · Pipeline ledger               │

└────────────────────────┬────────────────────────────────────────┘

│  Dataflow Gen2 (Power Query M)

▼

┌─────────────────────────────────────────────────────────────────┐

│              GOLD LAYER (Fabric Warehouse)                      │

│   dbo schema — 5 Dimension tables · 3 Fact tables              │

│   Star schema optimised for Power BI Direct Lake               │

└────────────────────────┬────────────────────────────────────────┘

│  Direct Lake



## Data Model
dim_date          dim_region
                   │                  
 dim_customer ─────┤                  
                   │                  
 dim_tower ────────┼──── fact_subscriptions
                   │──── fact_network_events
 dim_vendor        └──── fact_support_tickets


 ## ✨ Key Features

| Feature | Description |
|---------|-------------|
| **Incremental Load** | Watermark-based — first run pulls full history, subsequent runs pull only new/changed rows |
| **Data Quality** | 6–8 DQ rules per table — null checks, domain validation, referential integrity |
| **Delta MERGE** | Upsert semantics — updated source records replace existing Silver rows, no duplicates |
| **Pipeline Ledger** | Delta table tracking rows_in, rows_out, rows_rejected, status per table per run |
| **Star Schema** | 3 fact tables + 5 dimensions — surrogate keys, derived columns, FX conversion |
| **Audit Trail** | Per-table DQ JSON logs + consolidated pipeline ledger — full lineage every run |
| **Zero Hardcoded Secrets** | All credentials in JSON config loaded at runtime from Lakehouse Files section |


---

## 📦 Data Sources

| Source | Tool | Tables | Rows |
|--------|------|--------|------|
| MySQL | Aiven (free cloud) | customers, subscriptions, fx_rates | 25,404 |
| PostgreSQL | Supabase (free cloud) | towers, network_events, vendors | 100,700 |
| Google Sheets | Google Cloud API | support_tickets | 5,000 |
| **Total** | | **7 tables** | **131,104** |

---

## 🔧 Tech Stack

| Layer | Tool | Version |
|-------|------|---------|
| Platform | Microsoft Fabric | Trial E5 |
| Compute | PySpark | 3.5 |
| Storage | Delta Lake | 3.2 |
| Ingestion | Fabric Notebook | — |
| Transformation | Fabric Notebook | — |
| Gold ETL | Dataflow Gen2 | Power Query M |
| Warehouse | Fabric Warehouse | T-SQL |
| Orchestration | Fabric Data Pipeline | — |
| Data Generation | Python Faker | 24.0.0 |
| Source — MySQL | Aiven | MySQL 8.4 |
| Source — PostgreSQL | Supabase | PostgreSQL 15 |
| Source — Sheets | Google Cloud API | Sheets API v4 |




