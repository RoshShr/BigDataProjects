# 🚀 Azure Data Engineering: The Medallion Pipeline

![Project Status](https://img.shields.io/badge/Status-Active_Development-yellow?style=flat-square) ![Azure](https://img.shields.io/badge/Cloud-Azure-blue?style=flat-square) ![Security](https://img.shields.io/badge/Security-Key%20Vault-green?style=flat-square)

## 📖 Project Overview

This project implements a secure, enterprise-grade data pipeline using the **Databricks Medallion Architecture** (Bronze/Silver/Gold). The goal is to ingest raw data from on-premise SQL systems and APIs, process it using **Delta Live Tables (DLT)**, and model it into a **Star Schema** for high-performance warehousing.

The architecture prioritizes **security** (via Key Vault) and **efficiency** (via CDC & Incremental Loading).

---

## 🏗️ Architecture
<img width="1591" height="932" alt="Spotify Architecture" src="https://github.com/user-attachments/assets/5f47de3f-f320-48fd-9110-07064e7ad228" />


*(Note: This diagram illustrates the planned end-to-end flow)*

### Data Flow Breakdown
1.  **Ingestion (ADF):** Azure Data Factory extracts data from **Azure SQL** and **GitHub** and lands it in the **Bronze Layer** (Raw) of the Data Lake.
2.  **Security:** All credentials and connection strings are managed securely via **Azure Key Vault** to ensure zero secrets in the code.
3.  **Transformation (Medallion):**
    * **Bronze:** Raw historical data.
    * **Silver:** Cleaned and standardized data, modeled into a **Star Schema**.
    * **Gold:** Aggregated metrics ready for reporting.
4.  **Processing Engine:** Uses **Azure Databricks** and **Delta Live Tables** for reliable ETL pipelines.
5.  **Serving:** The Gold data is served via **Azure Synapse Analytics** for downstream reporting.

---

## 🛠️ Tech Stack

| Domain | Technology | Usage |
| :--- | :--- | :--- |
| **Cloud Provider** | Microsoft Azure | Hosting environment |
| **Orchestration** | Azure Data Factory (ADF) | Ingestion scheduling & CDC handling |
| **Storage** | ADLS Gen2 | Data Lake Storage (Bronze/Silver/Gold) |
| **Compute** | Azure Databricks | Spark & Delta Live Tables (DLT) |
| **Security** | Azure Key Vault | Secret management |
| **Warehousing** | Azure Synapse | SQL Data Warehouse |
| **Format** | Delta Lake | ACID compliant storage layer |

---

## 🌟 Key Technical Highlights

### 🔹 1. CDC & Incremental Loading | Implement Backfill to retrieve Historical Data when needed
**The Challenge:** Full data reloads are inefficient and costly for large SQL datasets.
**The Solution:**
* Implemented **Change Data Capture (CDC)** on the Azure SQL Source.
* Configured ADF pipelines to detect specific Date from JSON and only ingest dates greater than the last run.
* Configured Backfill Feature on existing pipeline to enable Historical Data access from any Date Range.
* **Impact:** Reduces daily data ingress volume by **95%** and ensures near real-time data freshness.

### 🔹 2. Security-First Design
Instead of hardcoding database passwords in the ADF pipeline, this project uses **Azure Key Vault**. The Data Factory retrieves secrets dynamically at runtime, adhering to the principle of Least Privilege.

### 🔹 3. Delta Live Tables (Planned)
The transformation layer is designed to use **Delta Live Tables (DLT)** to automatically manage infrastructure, data quality expectations, and dependency handling between the Bronze and Silver tables.

---

## 📂 Repository Structure

```bash
├── adf-pipelines/         # ARM Templates & JSON for Data Factory
├── sql-cdc-setup/         # SQL scripts to enable CDC on source DB
├── databricks-dlt/        # (In Progress) Python/SQL notebooks for Delta Live Tables
└── architecture/          # Diagrams and Design Docs
