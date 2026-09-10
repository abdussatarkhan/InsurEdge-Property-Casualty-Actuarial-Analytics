# 📚 Data Warehouse Data Dictionary

## 🏢 Platform: InsurEdge: Property & Casualty (P&C) Actuarial Reserving & Claims Analytics
**Schema**: `insuredge_dw`  
**Architecture**: 3-Tier Enterprise Star Schema (PostgreSQL 16)  
**SCD Strategy**: Slowly Changing Dimension Type 2 (SCD-2) on primary business entities

---

## 🗄️ Dimension Tables

### 1. `dim_date`
Calendar date dimension providing temporal hierarchical drill-down.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `date_key` | INT | NO | PK | Integer key in YYYYMMDD format (e.g. 20241205) |
| `full_date` | DATE | NO | UNIQUE | Standard ISO-8601 calendar date |
| `day_name` | VARCHAR(12) | NO | - | Day of week string (Monday..Sunday) |
| `is_weekend` | BOOLEAN | NO | - | True for Saturday and Sunday |
| `month` | INT | NO | - | Calendar month integer (1..12) |
| `quarter` | INT | NO | - | Calendar quarter (1..4) |
| `year` | INT | NO | - | 4-digit calendar year |

### 2. `dim_policyholders`
Dimensional attribute master for Policyholders.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `policyholders_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `policyholders_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

### 2. `dim_lines_of_business`
Dimensional attribute master for Lines Of Business.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `lines_of_business_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `lines_of_business_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

### 2. `dim_coverage_types`
Dimensional attribute master for Coverage Types.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `coverage_types_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `coverage_types_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

### 2. `dim_geographic_zones`
Dimensional attribute master for Geographic Zones.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `geographic_zones_key` | SERIAL / INT | NO | PK | Surrogate primary key |
| `geographic_zones_code` | VARCHAR(32) | NO | UNIQUE | Natural business identifier code |
| `name` | VARCHAR(100) | NO | - | Canonical entity name |
| `category` | VARCHAR(50) | NO | - | Operational classification category |
| `status_tier` | VARCHAR(20) | NO | - | Operational state (ACTIVE, INACTIVE, SUSPENDED) |
| `created_at` | TIMESTAMP | NO | - | System ingestion audit timestamp |

---

## 📊 Fact Tables

### `fact_policies_written`
Transactional and snapshot grain telemetry table tracking Policies Written.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

### `fact_claims_incurred`
Transactional and snapshot grain telemetry table tracking Claims Incurred.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

### `fact_loss_reserves_triangle`
Transactional and snapshot grain telemetry table tracking Loss Reserves Triangle.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

### `fact_catastrophe_losses`
Transactional and snapshot grain telemetry table tracking Catastrophe Losses.

| Column Name | Data Type | Nullable | Key | Description |
|---|---|:---:|:---:|---|
| `event_id` | BIGSERIAL | NO | PK | Surrogate transactional event identifier |
| `date_key` | INT | NO | FK | Foreign key referencing `dim_date(date_key)` |
| `metric_value_usd` | NUMERIC(12, 2) | NO | - | Monetary volume or financial impact ($ USD) |
| `operational_count` | INT | NO | - | Consignment volume, transaction count, or batch units |
| `latency_duration_mins` | NUMERIC(8, 2) | NO | - | Total execution latency / duration in minutes |
| `is_sla_compliant` | BOOLEAN | NO | - | True if duration satisfies enterprise SLA |
| `created_at` | TIMESTAMP | NO | - | Row insertion audit timestamp |

---

## 📈 Key Metric Calculation Formulas

1. **SLA Compliance Rate (%)**:
   $$\text{SLA Compliance} = \left( \frac{\sum \text{is\_sla\_compliant = TRUE}}{\text{Total Events}} \right) \times 100$$

2. **Rolling 7-Day Moving Average**:
   $$\text{SMA}_{7} = \frac{1}{7} \sum_{i=0}^{6} \text{metric\_value}_{t-i}$$

3. **Z-Score Anomaly Threshold**:
   $$Z = \frac{X - \mu}{\sigma}$$
   *(Alert flagged when $|Z| \ge 2.58$, representing $p < 0.01$ outlier probability)*
