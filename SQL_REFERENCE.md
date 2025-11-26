# SQL Dialect Reference Guide

Quick reference for syntax differences across MySQL, Oracle, PostgreSQL, and Presto/Trino/Athena.

---

## Type Casting

### CAST AS FLOAT
```sql
-- MySQL, Oracle, PostgreSQL
CAST('99.95' AS FLOAT)  -- 99.95
```

### CAST AS DOUBLE
```sql
-- MySQL
CAST('123.456' AS DOUBLE)  -- 123.456

-- Oracle
CAST('123.456' AS BINARY_DOUBLE)  -- 123.456

-- PostgreSQL
CAST('123.456' AS DOUBLE PRECISION)  -- 123.456
```

### CAST AS INTEGER
```sql
-- MySQL
CAST(99.99 AS SIGNED)  -- 100

-- Oracle, PostgreSQL
CAST(99.99 AS INT)  -- 100
```

### CAST AS TIME
```sql
-- MySQL
CAST('14:30:45' AS TIME)  -- 14:30:45

-- Oracle
TO_CHAR(SYSDATE, 'HH24:MI:SS')  -- '14:30:45'

-- PostgreSQL
CAST('14:30:45' AS TIME)  -- 14:30:45
```

### CAST AS TIMESTAMP
```sql
-- MySQL, Oracle, PostgreSQL
CAST('2024-01-15 14:30:45' AS TIMESTAMP)  -- 2024-01-15 14:30:45
```

---

## Date Functions

### MySQL
- **Convert to date**: `DATE(timestamp)`
- **Extract week**: `WEEK(date)`
- **Extract month**: `MONTH(date)`
- **TRUNC() does NOT work** - use `DATE()` instead
- **Format date/time**:
  ```sql
  DATE_FORMAT(timestamp, '%Y%m%d')     -- '20240101'
  DATE_FORMAT(timestamp, '%Y-%m-%d')   -- '2024-01-01'
  DATE_FORMAT(timestamp, '%H:%i:%s')   -- '14:23:45'
  DATE_FORMAT(timestamp, '%W')         -- 'Monday'
  ```

### Presto/Trino/Athena
- **Convert to date**: `DATE(timestamp)` or `CAST(timestamp AS DATE)`
- **Extract week**: `WEEK_OF_YEAR(date)`
- **Extract parts**: `EXTRACT(YEAR|MONTH|HOUR|DAY FROM timestamp)`
- **Parse string**: `DATE_PARSE('2024-11-21', '%Y-%m-%d')`
- **Truncate**: `DATE_TRUNC('day'|'week'|'month', timestamp)`
- **Format date/time**:
  ```sql
  DATE_FORMAT(DATE '2024-01-01', '%Y%m%d')     -- '20240101'
  DATE_FORMAT(DATE '2024-01-01', '%Y-%m-%d')   -- '2024-01-01'
  DATE_FORMAT(DATE '2024-01-01', '%H:%i:%S')   -- '14:23:45'
  DATE_FORMAT(DATE '2024-01-01', '%W')         -- 'Monday'
  ```

### PostgreSQL
- Similar to Presto, plus `TO_DATE(string, 'YYYY-MM-DD')`
- **Parse timestamp**: `TO_TIMESTAMP(string, 'YYYY-MM-DD"T"HH24:MI:SS')`
- **Truncate**: `TRUNC(timestamp)` -- Returns date portion
- **Use**: `PERCENTILE_CONT(0.7) WITHIN GROUP (ORDER BY col)`
- **Format date/time**:
  ```sql
  TO_CHAR(timestamp, 'YYYYMMDD')       -- '20240101'
  TO_CHAR(timestamp, 'YYYY-MM-DD')     -- '2024-01-01'
  TO_CHAR(timestamp, 'HH24:MI:SS')     -- '14:23:45'
  TO_CHAR(DATE '2024-01-01', 'Day')    -- 'Monday'
  TO_CHAR(DATE '2024-01-01', 'Month')  -- 'January'
  ```

---

## String & Other Functions

### String Concatenation
```sql
-- MySQL, PostgreSQL, Oracle
CONCAT(str1, str2)

-- Oracle, PostgreSQL only
str1 || str2 || str3
```

**Note**: Oracle's `CONCAT()` accepts only 2 arguments

### Cartesian Product
```sql
FROM table1 CROSS JOIN table2
```

### Window Functions
```sql
-- Previous row
LAG(column, offset, default) OVER (PARTITION BY group ORDER BY sort)

-- Aggregates
AGG_FUNC(column) OVER (PARTITION BY group1, group2 ORDER BY sort)

-- AGG_FUNC(): MIN() / MAX() / COUNT() / SUM() / AVG()
```

### Scalar Subquery
```sql
column / (SELECT AGG_FUNC(column) FROM table WHERE condition)
```
