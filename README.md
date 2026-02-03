# End-to-End E-commerce Analytics Pipeline on Azure

<div align="center">

**A fully automated cloud data pipeline that ingests, transforms, enriches and visualises Olist e-commerce data using Azure following the Medallion (Bronze → Silver → Gold) architecture.**

[![Azure](https://img.shields.io/badge/Azure-Cloud%20Platform-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/)
[![ADF](https://img.shields.io/badge/Azure%20Data%20Factory-Orchestration-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/data-factory/)
[![Databricks](https://img.shields.io/badge/Azure%20Databricks-PySpark-FF3621?logo=databricks&logoColor=white)](https://azure.microsoft.com/en-us/products/databricks/)
[![ADLS](https://img.shields.io/badge/ADLS%20Gen2-Data%20Lake-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/storage/data-lake-storage/)
[![Synapse](https://img.shields.io/badge/Azure%20Synapse-Analytics-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/synapse-analytics/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Enrichment-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![PowerBI](https://img.shields.io/badge/Power%20BI-Visualization-F2C80F?logo=microsoftpowerbi&logoColor=white)](https://www.microsoft.com/en-us/power/power-bi)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

</div>

## Architecture Diagram
![architecture_diagram](https://github.com/Yashraj-Muthyapwar/End-to-End-E-commerce-Analytics-Pipeline-on-Azure/blob/main/Architecture.png) 


## 🛠️ Tech Stack

| Service | Role |
|---|---|
| Azure Data Factory | Ingestion & orchestration |
| ADLS Gen2 | Bronze & Gold layer storage |
| Azure Databricks (PySpark) | Transformation & enrichment |
| MongoDB | Reference / lookup tables |
| Azure Synapse Analytics | Serverless SQL serving |
| Power BI / Fabric | Dashboards & reports |

## 🔄 Pipeline Flow

```
Data Sources (HTTP / SQL)
        ↓  Azure Data Factory
   ADLS Gen2 (Bronze — Raw Data)
        ↓
   Azure Databricks ← MongoDB (Enrichment)
        ↓  PySpark Transformation
   ADLS Gen2 (Gold — Transformed Data)
        ↓
   Azure Synapse Analytics
        ↓
   Power BI / Fabric (Dashboards)
```
## Prerequisites

- **Azure Subscription** — Active subscription with resource creation permissions
- **Git** — For version control and ADF Git integration
- **Power BI Desktop** — For dashboard visualization
- **filess.io Setup** — Free tier for SQL & MongoDB data hosting
  
## ▶️ How to Run

1. Deploy Azure resources (Data Factory, ADLS Gen2, Databricks, Synapse)
2. Publish the ADF pipelines from the `factory/` and `pipeline/` configs
3. Connect linked services using `linkedService/` definitions
4. Trigger the ADF ingestion pipeline
5. Run Databricks notebooks in order:
   - `DataIngestionToSQL.ipynb`
   - `DataIngestionToNoSQL.ipynb`
6. Query the gold layer in **Synapse Studio** using scripts in `synapse/`
7. Connect **Power BI** to Synapse and open dashboards

> 💡 All secrets and credentials are managed inside Azure no local `.env` files needed.



### 📝 License
This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for more details.

Contributions welcome built with ❤️ on Azure to make e-commerce querying & insights feel effortless.


