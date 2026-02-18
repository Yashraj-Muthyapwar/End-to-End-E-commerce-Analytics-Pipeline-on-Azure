<div align="center">

# End-to-End E-commerce Analytics Pipeline on Azure

**Automated cloud data pipeline that ingests, transforms, enriches, and visualises Olist e‑commerce data using Azure — following the Medallion (Bronze → Silver → Gold) architecture.**

[![Azure](https://img.shields.io/badge/Cloud-Azure-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![ADF](https://img.shields.io/badge/Orchestration-Data%20Factory-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/data-factory/)
[![Databricks](https://img.shields.io/badge/Transform-Databricks-FF3621?logo=databricks&logoColor=white)](https://azure.microsoft.com/en-us/products/databricks/)
[![ADLS](https://img.shields.io/badge/Storage-ADLS%20Gen2-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/storage/data-lake-storage/)
[![Synapse](https://img.shields.io/badge/Serving-Synapse%20Analytics-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/synapse-analytics/)
[![MongoDB](https://img.shields.io/badge/Enrichment-MongoDB-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![PowerBI](https://img.shields.io/badge/Viz-Power%20BI-F2C80F?logo=microsoftpowerbi&logoColor=white)](https://www.microsoft.com/en-us/power/power-bi)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

</div>

## 💡 Problem

An e-commerce company (Olist) has raw transactional data scattered across multiple sources — HTTP endpoints, SQL databases, and NoSQL stores. Analysts cannot run reliable queries because data is unclean, denormalized, and siloed.

**Solution:** This project solves that by implementing a fully automated **Medallion Architecture on Azure** that unifies ingestion, transformation, and analytics.

| Layer | What Happens |
|-------|-------------|
| **Bronze** | **ADF** ingests raw data from **HTTP, SQL,** and **MongoDB** into **ADLS Gen2** |
| **Silver** | **Databricks (PySpark)** cleans, deduplicates, and standardizes schemas |
| **Gold** | Enriched, analytics-ready tables served via **Synapse** for **Power BI dashboards** |

## 🏛️ Architecture

![Architecture Diagram](https://github.com/Yashraj-Muthyapwar/End-to-End-E-commerce-Analytics-Pipeline-on-Azure/blob/main/Architecture.png)


## 🛠️ Tech Stack

| Layer | Service | Purpose |
|-------|---------|---------|
| Ingestion | Azure Data Factory | Ingestion & orchestration — Copy pipelines from HTTP, SQL, MongoDB |
| Storage | ADLS Gen2 | Data lake — Bronze, Silver and Gold layer storage|
| Transformation | Azure Databricks (PySpark) | Cleaning, deduplication, schema standardization |
| Enrichment | MongoDB | Reference/lookup data joined during transformation |
| Serving | Azure Synapse Analytics | Serverless SQL pool for ad-hoc querying |
| Visualization | Power BI / Fabric | Executive dashboards and operational reports |


## 📁 Project Structure

```
azure-ecommerce-pipeline/
├── factory/                          # ADF factory configuration
├── pipeline/                         # ADF pipeline definitions
│   ├── ingest_http_pipeline.json     # HTTP → Bronze
│   ├── ingest_sql_pipeline.json      # SQL → Bronze
│   └── ingest_mongo_pipeline.json    # MongoDB → Bronze
├── linkedService/                    # ADF linked service connections
├── dataset/                          # ADF dataset definitions
├── notebooks/
│   ├── DataIngestionToSQL.ipynb      # SQL source ingestion
│   └── DataIngestionToNoSQL.ipynb    # MongoDB ingestion & enrichment
├── synapse/                          # Synapse SQL scripts for Gold layer
├── Architecture.png                  # Architecture diagram
└── README.md
```

## 🔑 Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Medallion architecture** | Clear data quality progression — Bronze (raw) → Silver (cleaned) → Gold (business-ready) |
| **ADF for orchestration** | Native Azure integration, visual pipeline builder, built-in monitoring |
| **MongoDB for enrichment** | NoSQL flexibility for semi-structured reference data that doesn't fit a relational schema |
| **ADLS Gen2 over Blob Storage** | Hierarchical namespace for efficient file operations + fine-grained ACLs |
| **Synapse serverless SQL** | Pay-per-query — no dedicated cluster costs for intermittent analyst queries |
| **PySpark on Databricks** | Scalable distributed processing for large datasets + Delta Lake support |

## 🚀 Getting Started

### Prerequisites

- Azure Subscription with resource creation permissions
- Git (for ADF Git integration)
- Power BI Desktop

### Deployment

```bash
# 1. Deploy Azure resources
#    Data Factory, ADLS Gen2, Databricks workspace, Synapse workspace

# 2. Publish ADF pipelines
#    Import factory/, pipeline/, linkedService/, dataset/ configs

# 3. Trigger ADF ingestion pipeline
#    ADF → Monitor → Pipeline Runs

# 4. Run Databricks notebooks (in order)
#    DataIngestionToSQL.ipynb → DataIngestionToNoSQL.ipynb

# 5. Query Gold layer in Synapse Studio
#    Use scripts from synapse/

# 6. Connect Power BI to Synapse → build dashboards
```

> 💡 All secrets and credentials are managed inside Azure — no local `.env` files needed.

## 📝 License
This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for more details.

## 🌟 About Me

Hi there! I'm **Yashraj Muthyapwar** a Data Engineer passionate about building scalable, cloud-native data systems. This project demonstrates my hands-on experience with Azure's modern data platform, end-to-end pipeline design, and the Medallion architecture pattern.
 
> ### 🌟 Contributions Welcome  
> Built with ❤️ on Azure to make e-commerce analytics feel effortless.
