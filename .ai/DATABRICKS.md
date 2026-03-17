# Databricks Working Conventions

This document captures how we should structure, name, and manage data assets in Databricks.

Source basis:
- https://medium.com/@valentin.loghin/databricks-best-practices-and-naming-conventions-105013ec31c9

## Why this exists

Use consistent Databricks conventions to improve:
- clarity across teams
- onboarding speed
- governance and lineage
- pipeline maintainability
- environment safety (dev/test/prod separation)

## Core object model

Always organize Unity Catalog objects as:

```text
<catalog>.<schema>.<table>
```

Use:
- `catalog` for environment or domain boundaries
- `schema` for processing layers and logical grouping
- `table` for the concrete dataset object

## Unity Catalog conventions

Preferred schema layers:
- `bronze` (raw/landing data)
- `silver` (cleansed/enriched data)
- `gold` (business/reporting-ready data)
- optional supporting schemas: `staging`, `reference`, `data_vault`, `mart`/`marts`

Canonical example:

```text
sales.bronze.customer_raw
sales.silver.customer_cleaned
sales.gold.customer_orders_scd2
```

## Naming conventions

General rules:
- use `lower_snake_case` for tables and models
- keep names short, explicit, and domain-oriented
- encode semantic intent in names (raw, hub, sat, link, dim, fact, scd2)

Object-level naming matrix:

| Object | Convention | Example |
|---|---|---|
| Catalog | `lower_snake_case` | `sales`, `finance`, `dev_catalog` |
| Schema | `lower_snake_case` | `bronze`, `silver`, `data_vault` |
| Table | `lower_snake_case`, descriptive | `customer_raw`, `hub_customer`, `fact_sales` |
| Column | `lower_snake_case` | `customer_id`, `order_date` |
| Notebook | `<layer>_<purpose>` | `bronze_ingest_customer`, `gold_report_generation` |
| dbt Model | `<layer>__<entity>` | `silver__customer`, `gold__fact_orders` |
| Function / View | `fn_<purpose>` or `vw_<purpose>` | `vw_customer_summary` |

By layer/modeling style:
- raw landing: `<entity>_raw` (example: `customer_raw`)
- curated/cleaned: `<entity>_cleaned` or similarly explicit cleaned-state name
- Data Vault hub: `hub_<business_key_entity>` (example: `hub_customer`)
- Data Vault satellite: `sat_<entity>_<context>` (example: `sat_customer_profile`)
- Data Vault link: `link_<entity1>_<entity2>` (example: `link_order_customer`)
- dimensional dimension: `dim_<entity>[_scd2]` (example: `dim_customer_scd2`)
- dimensional fact: `fact_<process_or_event>` (example: `fact_sales_orders`)

## dbt conventions in Databricks

Model naming:
- must follow `lower_snake_case`
- should mirror Databricks layer and modeling semantics

Folder layout by layer:

```text
models/
  bronze/
  silver/
  gold/
  data_vault/
  marts/
```

Representative project shape:

```text
my_dbt_project/
  models/
    bronze/
      customer_raw.sql
      orders_raw.sql
    silver/
      hub_customer.sql
      sat_customer_profile.sql
      link_order_customer.sql
    gold/
      dim_customer_scd2.sql
      fact_sales_orders.sql
    marts/
      sales_summary.sql
      customer_reports.sql
  dbt_project.yml
  profiles.yml
```

## External (unmanaged) tables

When storing data in external cloud storage, define tables with an explicit location instead of relying on DBFS mounts.

SQL pattern:

```sql
CREATE TABLE sales.silver.customer_cleaned
USING PARQUET
LOCATION 'abfss://datalake@youraccount.dfs.core.windows.net/silver/customer_cleaned';
```

dbt pattern:

```yaml
models:
  my_project:
    silver:
      +materialized: table
      +location_root: "/mnt/external/silver"
```

```jinja
{{ config(
    materialized="table",
    location="abfss://datalake@youraccount.dfs.core.windows.net/silver/customer_cleaned",
    unmanaged=True
) }}
```

## Storage path convention

Use a predictable ADLS path pattern:

```text
abfss://<container>@<storage_account>.dfs.core.windows.net/<env>/<layer>/<domain>/<table>/
```

Example:

```text
abfss://datalake@mycompany.dfs.core.windows.net/prod/bronze/sales/customer/
```

## Environment management

Rules:
- keep `dev`, `test`, and `prod` isolated via separate catalogs or workspaces
- avoid mixing environments in one catalog
- if shared cataloging is unavoidable, include env prefix in the catalog/domain identifier (example: `dev_sales`)

Example FQN with env prefix:

```text
dev_sales.gold.customer_orders
```

## Suggested `dbt_project.yml` baseline

```yaml
name: "my_dbt_project"
version: "1.0"
config-version: 2
profile: "databricks_prod"
model-paths: ["models"]

models:
  my_dbt_project:
    bronze:
      +schema: bronze
      +materialized: view
    silver:
      +schema: silver
      +materialized: table
      +unmanaged: true
      +location: "abfss://datalake@storageaccount.dfs.core.windows.net/prod/silver/sales/"
    gold:
      +schema: gold
      +materialized: table
    marts:
      +schema: marts
      +materialized: view
```

## AI usage notes

When generating Databricks code, SQL, dbt models, or architecture docs, the AI should:
- always emit fully qualified names as `<catalog>.<schema>.<table>`
- place assets into the correct layer (`bronze`/`silver`/`gold`)
- follow `lower_snake_case` consistently
- apply Data Vault prefixes (`hub_`, `sat_`, `link_`) where relevant
- apply dimensional prefixes (`dim_`, `fact_`) where relevant
- include explicit `abfss://...` locations for unmanaged external tables
- preserve strict environment separation

## Summary of best practices

| Area | Best practice |
|---|---|
| Naming | Use `lower_snake_case`, clear prefixes (`hub_`, `sat_`, `dim_`, `fact_`) |
| Data Layering | Use `bronze`, `silver`, `gold`, or `raw`, `cleansed`, `curated` |
| Catalogs | Separate by environment or domain |
| Schemas | Organize by layer (`bronze`, `silver`, `data_vault`, etc.) |
| Tables | Use purpose-descriptive names, avoid ambiguity |
| File Paths | Use a predictable, layered directory structure in storage |
| dbt Integration | Use `location` + `unmanaged=True` for external tables |

## Confidence note

The source article includes image-based sections. This document now includes those conventions from provided screenshots, plus machine-readable article content.
