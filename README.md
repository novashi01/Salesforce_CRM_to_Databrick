# Salesforce CRM Data Pipeline in Databricks

This project implements an end-to-end data pipeline to extract data from Salesforce CRM, process it within Databricks using Delta Live Tables (DLT), and transform it into analysis-ready datasets following the Medallion Architecture (Bronze, Silver, Gold layers).

## Purpose

The primary goal is to create reliable, cleaned, and aggregated datasets from core Salesforce objects (like Account, Lead, Opportunity) suitable for business intelligence, reporting, and advanced analytics within the Databricks Lakehouse Platform.

## Key Features

*   Processes key Salesforce CRM objects (Account, Lead, Opportunity, Opportunity Product, User).
*   Follows the Medallion Architecture:
    *   **Bronze:** Raw data landing.
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
    *   **Assumption:** These tables are populated by a separate process before the DLT pipeline runs. This pipeline *consumes* Bronze data.

2.  **Silver Layer:**
    *   Cleans raw data: casting data types, trimming strings, handling nulls.
    *   Standardizes column names.
    *   Applies basic data quality rules using DLT `EXPECT` constraints.
    *   Creates core entity tables like `silver_account`, `silver_lead`, `silver_opportunity`, etc.
    *   **Output Schema:** Configured in the DLT pipeline settings.

3.  **Gold Layer:**
    *   Reads from **Silver layer tables**
    *   Performs aggregations, joins, and denormalization to create business-centric views.
    *   `gold_opportunity_snapshot`: A wide table with one row per opportunity, enriched with account, owner, owner's manager, and aggregated line item data. Powers detailed opportunity analysis.
    *   `gold_lead_funnel`: Tracks lead progression, conversion details, and potentially the converted account's owner.
    *   `gold_sales_pipeline_agg`: Aggregates won revenue, deal counts, win rates, etc., by month, manager, owner, industry, and other dimensions.
    *   `gold_lead_source_agg`: Aggregates lead creation, conversion counts, and rates by month, lead source, etc.
    *   `gold_monthly_product_sales`: Aggregates product sales revenue and quantity by month and product name.

## Dashboarding

The Gold layer tables are designed to be directly queried by BI tools or Databricks SQL Dashboards to create visualizations such as:

*   Sales Funnel Performance (KPIs for leads created, converted, opportunities won)
*   Current Pipeline Stage Distribution
*   Monthly Closed Won Opportunities (Trend)
*   Monthly Lead Creation vs. Conversion (Trend)
*   Won vs. Total Closed Deals by Owner (Count & Ratio)
*   Team Performance by Manager (Revenue)
*   Revenue Distribution by Customer Industry
*   Product Contribution to Monthly Revenue


