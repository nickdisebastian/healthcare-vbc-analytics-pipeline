# Healthcare VBC Analytics Pipeline

## Overview

This project demonstrates an end-to-end healthcare data pipeline designed to support value-based care (VBC) analytics. Using publicly available synthetic Medicare claims data, the pipeline ingests raw files into Google Cloud Storage, loads and models the data in BigQuery using dbt, orchestrates workflows with Apache Airflow, and exposes analytics-ready metrics through a Looker semantic layer.

The project also incorporates **Claude via Vertex AI** in two complementary ways:
1. As a **development-time code assistant** within the IDE to accelerate writing and reviewing pipeline code.
2. As an **optional, lightweight pipeline step** that generates natural-language summaries from aggregated VBC metrics for stakeholder-friendly reporting.

## Tooling

### Core Platform
- Google Cloud Storage (GCS)
- BigQuery

### Orchestration
- Apache Airflow (local via Docker)

### Ingestion
- Python (requests, pandas, pyarrow)
- CSV to Parquet conversion for efficient downstream processing

### Transformations and Modeling
- dbt Core (BigQuery adapter)

### Analytics and BI
- Looker (semantic layer and dashboards)

### AI Integration
- Claude via Vertex AI

### CI and Quality
- GitHub Actions
- dbt tests
- Python unit tests (pytest)

## Claude on Vertex AI: How It Is Used

Claude is intentionally used in two distinct contexts in this project.

### Development-Time Code Assistant

Claude is used within the IDE as a code agent to assist with:
- Writing and reviewing Python ingestion scripts
- Drafting and refining Airflow DAGs
- Developing dbt models and tests
- Improving SQL clarity and correctness

Claude is accessed through Vertex AI, using GCP authentication rather than static API keys. This mirrors how enterprise teams centrally govern and secure LLM access while still enabling developer productivity.

### Pipeline-Time Narrative Insights

As a small, visible example of operationalizing LLMs, the pipeline optionally includes an Airflow task that:
- Queries aggregated VBC metrics from BigQuery
- Sends a constrained prompt to Claude via Vertex AI
- Writes short narrative summaries back to BigQuery

This step operates only on aggregated data and is designed to be low-cost, auditable, and stakeholder-friendly.

## Data Source

- CMS Synthetic Medicare Claims (DE-SynPUF)

Public, de-identified data that simulates real Medicare claims and beneficiary information.

Only synthetic data is used. No PHI or sensitive data is included in this project.

## Semantic Layer Metrics

The semantic layer (Looker) is designed around a **member-month grain**, which is standard for healthcare and value-based care analytics. Metrics are modeled to be safe for aggregation, clearly defined, and directly aligned with common VBC reporting needs.

### Cost Metrics
- Total paid amount
- Per Member Per Month (PMPM) cost
- Inpatient paid amount
- Outpatient paid amount
- Professional (carrier) paid amount
- Cost by service category

These metrics support analysis of overall spend trends, cost drivers, and shifts in site-of-care utilization.

### Utilization Metrics
- Inpatient admission count
- Inpatient admission rate (per 1,000 members)
- Average length of stay
- Outpatient visit count
- Professional claim count
- Utilization per 1,000 members

These metrics help identify changes in care patterns, avoidable utilization, and opportunities for intervention.

### Population Context Metrics
- Member count
- Member months
- Average age
- Age band distribution
- Sex distribution
- Chronic condition indicators (derived from beneficiary data)

Population context metrics provide necessary denominators and segmentation for interpreting cost and utilization trends.

### High-Cost Member Indicators
- High-cost member flag (e.g., top percentile by spend)
- High-cost member count
- Percentage of total spend attributable to high-cost members

These measures support care management prioritization and executive-level reporting common in VBC organizations.

All metrics are derived from curated dbt models and exposed through a semantic layer designed to support consistent, stakeholder-friendly analysis.


## Project Goals

- Demonstrate production-minded data engineering practices
- Showcase healthcare and VBC domain understanding
- Build a clear, readable portfolio project for data engineering and analytics engineering roles
- Show thoughtful, governed use of AI in both development and analytics workflows