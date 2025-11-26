# SQL
Sample code for SQL

# Sales Analytics SQL Repository

Production SQL queries for analyzing buyer behavior, delivery performance, and transaction patterns across Oracle and Presto/Trino databases.

---

## Database Systems

| Query                          | Database            |
|--------------------------------|---------------------|
| buyer_secondhand_analysis      | Oracle              |
| transaction_value_distribution | Oracle              |
| delivery_performance           | Presto/Trino/Athena |


---

## 📋 Files Overview

### 1️⃣ `buyer_secondhand_analysis.sql` (Oracle)
**Purpose**: Identify high-value repeat buyers with 90%+ second-hand purchase ratio

**Key Outputs**:
- Buyer demographics (product class, gender, age range)
- Total GMV vs. second-hand GMV
- Repurchase behavior metrics

**Filters**:
- Period: Jan-Apr 2023
- Min GMV: 10,000
- Second-hand ratio: ≥90%
- Product classes: 0008, 0011, 0021, 0022, 0023


---

### 2️⃣ `transaction_value_distribution.sql` (Oracle)
**Purpose**: Daily distribution of transaction values by range (Jan-May 2024)

**Key Outputs**:
- Transaction counts by value tier (≤500, 501-1000, 1001+)
- Daily percentage distribution within each date
- Excludes records already in archive

**Filters**:
- Period: Jan 1 - May 30, 2024 (151 days)
- Tags: 'mid' or 'large' only
- Excludes transactions in `record` table
- Only dates where value range >0.1% of daily total

---

### 3️⃣ `delivery_performance.sql` (Presto/Trino)
**Purpose**: Regions exceeding 70th percentile for international delivery GMV ratio

**Key Outputs**:
- Daily/weekly/monthly GMV aggregations
- International vs. total delivery GMV
- Region-month ratios

**Filters**:
- Valid delivery methods only
- Top 30% regions by international delivery share

---

## Key Definitions

| Term | Definition |
|------|------------|
| **Second-hand Goods** | `condition ≠ 'B'` AND name contains 'used' or 'second_handed' |
| **GMV** | Gross Merchandise Value = `amount × quantity` |
| **Age Ranges** | 10-19, 20-29, Other (based on birth year) |
| **Value Ranges** | ≤500, 501-1000, 1001+ |
| **Anti-join Pattern** | LEFT JOIN + IS NULL filter to exclude matching records |
