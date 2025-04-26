# Salesforce CRM Data Pipeline in Databricks

This project implements an end-to-end data pipeline to extract data from Salesforce CRM, process it within Databricks using Delta Live Tables (DLT), and transform it into analysis-ready datasets following the Medallion Architecture (Bronze, Silver, Gold layers).

## Purpose

The primary goal is to create reliable, cleaned, and aggregated datasets from core Salesforce objects (like Account, Lead, Opportunity) suitable for business intelligence, reporting, and advanced analytics within the Databricks Lakehouse Platform.

## Key Features

*   Processes key Salesforce CRM objects (Account, Lead, Opportunity, Opportunity Product, User).
*   Follows the Medallion Architecture:
    *   **Bronze:** Raw data landing (Assumed to be populated, e.g., by Fivetran, Airbyte, or custom scripts).
    *   **Silver:** Cleaned, standardized, and validated base tables.
    *   **Gold:** Aggregated, denormalized, business-centric views ready for analysis.
*   Utilizes **Delta Live Tables (DLT)** for declarative pipeline definition, data quality enforcement, and dependency management.
*   Written primarily in **SQL** for DLT pipeline definition.

## Technologies Used

*   **Data Source:** Salesforce CRM
*   **Platform:** Databricks Lakehouse Platform
*   **Processing Framework:** Delta Live Tables (DLT)
*   **Storage Format:** Delta Lake
*   **Language:** Python / SQL
*   **Version Control:** Git / GitHub

## Pipeline Stages (Medallion Architecture)

1.  **Bronze Layer:**
    *   Contains raw data directly reflecting the source Salesforce objects.
    *   **Assumption:** These tables are populated by a separate process (e.g., Fivetran, Airbyte, custom ingestion scripts) before the DLT pipeline runs. This pipeline *consumes* Bronze data.

2.  **Silver Layer:**
    *   Cleans raw data: casting data types, trimming strings, handling nulls.
    *   Standardizes column names (e.g., removing spaces, using snake_case).
    *   Applies basic data quality rules using DLT `EXPECT` constraints.
    *   Creates core entity tables like `silver_account`, `silver_lead`, `silver_opportunity`, etc.
    *   **Output Schema:** Configured in the DLT pipeline settings.

3.  **Gold Layer:**
    *   Reads from **Silver layer tables** (either using `live.` prefix if in a single pipeline, or physical table names if using segmented pipelines).
    *   Performs aggregations, joins, and denormalization to create business-centric views.
    *   Examples: `gold_opportunity_snapshot`, `gold_lead_funnel`, `gold_sales_pipeline_agg`.
    *   **Output Schema:** Configured in the DLT pipeline settings (e.g., `aitest.crm_gold` if using segmented pipelines, or the same target schema as Silver if using a single pipeline, relying on `gold_` prefix for distinction).



