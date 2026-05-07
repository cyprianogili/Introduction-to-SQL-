# Introduction-to-SQL-
A complete beginner's guide to SQL — from understanding databases to writing real queries on live data.



## Table of Contents
- [What is SQL?](#what-is-sql)
- [Types of Databases](#types-of-databases)
- [Why SQL Matters](#why-sql-matters)
- [Core Concepts](#core-concepts)
- [What Can SQL Do?](#what-can-sql-do)
- [Setting Up](#setting-up)
- [Basic SQL Commands](#basic-sql-commands)
- [Subqueries](#subqueries)
- [Connecting to a Real Server](#connecting-to-a-real-server)

---

## What is SQL?

**SQL (Structured Query Language)** is a standard programming language used to interact with **relational databases** — systems that store data in organized tables of rows and columns, just like a spreadsheet.

Think of it this way:

> A database stores your data. SQL is how you ask for it, change it, or delete it.

---

## Types of Databases

| Type | Structure | Examples |
|------|-----------|---------|
| **Relational Databases** | Tables with rows and columns | MySQL, PostgreSQL, SQLite, Oracle |
| **NoSQL Databases** | Flexible — key-value, document, column, or graph | MongoDB, Redis, Cassandra |
| **Cloud Databases** | Can be relational or NoSQL, hosted in the cloud | Amazon RDS, Google BigQuery, Azure SQL |

> SQL is mainly used with **Relational Databases.**

---

## Why SQL Matters

| Reason | What it means |
|--------|--------------|
| **Universal Skill** | Nearly every company that works with data uses SQL |
| **Efficient Retrieval** | Extract exactly what you need from millions of records in seconds |
| **Data Analysis** | Foundation of every data science project |

---

## Core Concepts

### Database
A **database** is like a digital filing cabinet — an organized collection of data stored on a computer.

### Table
A **table** is like a spreadsheet — it is the actual structure where data lives.

### Columns & Rows

| Concept | Also Called | What it means |
|---------|-------------|--------------|
| **Column** | Field | A category of information (e.g. Name, Age, City) |
| **Row** | Record | One complete entry of data |

### Example — A `students` Table:

| id | name | age | city |
|----|------|-----|------|
| 1 | Chidi | 20 | Lagos |
| 2 | Amaka | 22 | Abuja |
| 3 | Tunde | 19 | Lagos |

---

## What Can SQL Do?

SQL lets you fully manipulate databases:

| # | Action | SQL Command | What it does |
|---|--------|-------------|-------------|
| 01 | Retrieve | `SELECT` | Pull data from a database |
| 02 | Insert | `INSERT` | Add new records |
| 03 | Update | `UPDATE` | Modify existing records |
| 04 | Delete | `DELETE` | Remove records |
| 05 | Create | `CREATE` | Build new databases and tables |

---

## Setting Up

### Import Libraries

```python
import pandas as pd
import sqlite3
```

| Library | Purpose |
|---------|---------|
| `pandas` | Load, clean and analyze data as a DataFrame |
| `sqlite3` | Create and interact with a SQLite database from Python |

### Connect to a Database

```python
conn = sqlite3.connect("data.db")
```

> If `data.db` exists — it opens it. If it does not exist — it creates it automatically.

### Check What Tables Exist

```python
pd.read_sql_query(
    "SELECT name FROM sqlite_master WHERE type='table';",
    conn
)
```

> Always check what tables exist **before** you start writing queries — just like opening a CSV to see what columns you have.

---

## Basic SQL Commands

### 1. SELECT + FROM — Get Specific Columns

```sql
SELECT * FROM car_prices LIMIT 5;
```

| Part | What it means |
|------|--------------|
| `SELECT *` | Give me ALL columns (`*` means everything) |
| `FROM car_prices` | From the table called car_prices |
| `LIMIT 5` | Show only the first 5 rows |

> `LIMIT 5` is the same as `df.head()` in pandas.

To select specific columns only:

```sql
SELECT make, model, sellingprice FROM car_prices LIMIT 10;
```

---

### 2. WHERE — Filter Rows

```sql
SELECT make, model, sellingprice 
FROM car_prices 
WHERE sellingprice > 30000 
LIMIT 5;
```

| Symbol | Meaning | Example |
|--------|---------|---------|
| `>` | Greater than | `sellingprice > 30000` |
| `<` | Less than | `sellingprice < 10000` |
| `>=` | Greater than or equal | `sellingprice >= 30000` |
| `<=` | Less than or equal | `sellingprice <= 10000` |
| `=` | Equal to | `make = 'BMW'` |
| `!=` | Not equal to | `make != 'BMW'` |

> `WHERE` in SQL is the same as `df[df['col'] > value]` in pandas.

---

### 3. ORDER BY — Sort Results

```sql
SELECT make, model, sellingprice 
FROM car_prices 
ORDER BY sellingprice DESC 
LIMIT 5;
```

| Keyword | Meaning | Result |
|---------|---------|--------|
| `DESC` | Descending | Highest to Lowest |
| `ASC` | Ascending | Lowest to Highest |

> `ORDER BY` in SQL is the same as `sort_values()` in pandas.

---

### 4. DISTINCT — Unique Values

```sql
SELECT DISTINCT make FROM car_prices;
```

> `DISTINCT` removes duplicates — it shows each value **only once.**

> `DISTINCT` in SQL is the same as `df['col'].unique()` in pandas.

---

### 5. COUNT() — Count Rows

```sql
SELECT COUNT(*) AS total_cars FROM car_prices;
```

> `AS` renames the result column to something readable.

```
Without AS  →  column named  COUNT(*)     ugly and confusing
With AS     →  column named  total_cars   clean and clear
```

> `COUNT()` in SQL is the same as `len(df)` in pandas.

---

### 6. Aggregate Functions — SUM, AVG, MIN, MAX

```sql
SELECT 
    SUM(sellingprice) AS total_revenue,
    AVG(sellingprice) AS average_price,
    MIN(sellingprice) AS cheapest_car,
    MAX(sellingprice) AS most_expensive
FROM car_prices;
```

| SQL | What it does | Pandas Equivalent |
|-----|-------------|------------------|
| `SUM()` | Adds up all values | `df['col'].sum()` |
| `AVG()` | Calculates the average | `df['col'].mean()` |
| `MIN()` | Returns the smallest value | `df['col'].min()` |
| `MAX()` | Returns the largest value | `df['col'].max()` |

---

### 7. GROUP BY + AVG() — Aggregate by Category

```sql
SELECT make, AVG(sellingprice) AS avg_price
FROM car_prices
GROUP BY make
ORDER BY avg_price DESC
LIMIT 5;
```

> `GROUP BY` groups all rows with the same value together, then performs a calculation on each group.

> `GROUP BY` in SQL is the same as `df.groupby()` in pandas.

---

### 8. Combined — Filter, Group and Sort

```sql
SELECT make, model, AVG(sellingprice) AS avg_price
FROM car_prices
WHERE year >= 2015
GROUP BY make, model
ORDER BY avg_price DESC
LIMIT 10;
```

### The Order SQL Commands Always Follow:

```sql
SELECT        --  1. What columns do you want?
FROM          --  2. Which table?
WHERE         --  3. Filter which rows?
GROUP BY      --  4. Group by what?
ORDER BY      --  5. Sort by what?
LIMIT         --  6. How many rows to show?
```

> Always follow this order — never mix them up!

---

### Full SQL vs Pandas Reference:

| SQL Command | What it does | Pandas Equivalent |
|-------------|-------------|------------------|
| `SELECT *` | Get all columns | `df[]` |
| `SELECT col` | Get specific columns | `df[['col']]` |
| `WHERE` | Filter rows | `df[df['col'] > value]` |
| `ORDER BY` | Sort results | `sort_values()` |
| `LIMIT` | Show few rows | `head()` |
| `DISTINCT` | Remove duplicates | `unique()` |
| `COUNT()` | Count rows | `len(df)` |
| `SUM()` | Add up values | `df['col'].sum()` |
| `AVG()` | Find average | `df['col'].mean()` |
| `MIN()` | Find smallest | `df['col'].min()` |
| `MAX()` | Find largest | `df['col'].max()` |
| `GROUP BY` | Group and summarize | `groupby()` |

---

## Subqueries

A **subquery** is simply a query inside another query.

> Ask one question first, then use that answer to ask a better question.

```sql
SELECT make, model, sellingprice
FROM car_prices
WHERE sellingprice > (SELECT AVG(sellingprice) FROM car_prices);
```

### How It Runs:

```
Step 1 — Inner query runs FIRST    (inside the brackets)
               ↓
         SELECT AVG(sellingprice) FROM car_prices
         Calculates the average price
               ↓
Step 2 — Outer query runs SECOND   (uses that answer)
               ↓
         WHERE sellingprice > (that average)
               ↓
Step 3 — Returns all cars above average price
```

> The subquery always lives inside brackets `()`

---

## Connecting to a Real Server

### Install pymssql

```python
pip install pymssql
```

### Connect to Microsoft SQL Server

```python
import pymssql

conn = pymssql.connect(
    server   = "2.tcp.eu.ngrok.io",
    port     = 12426,
    user     = "Hartz",
    password = 'StrongPassword123!',
    database = "stock",
)
```

| Parameter | What it means |
|-----------|--------------|
| `server` | The address of the server on the internet |
| `port` | The specific door number to enter the server |
| `user` | Your username to log in |
| `password` | Your password to log in |
| `database` | The specific database you want to use |

### SQLite vs SQL Server:

| | SQLite | SQL Server |
|-|--------|-----------|
| **Connection** | `sqlite3.connect("data.db")` | `pymssql.connect(...)` |
| **Location** | Your computer | Internet server |
| **Needs login?** | No | Yes |
| **Needs password?** | No | Yes |
| **Real world?** | Practice | Real company style |

### Query a Real Stock Table

```python
query = "SELECT * FROM dbo.AAPL"
df2 = pd.read_sql(query, con=conn)
```

| Part | What it means |
|------|--------------|
| `dbo` | Schema — like a folder inside the database |
| `AAPL` | Table name — Apple Inc. stock data |
| `pd.read_sql` | Pandas function that works with any database |

> `AAPL` is Apple Inc.'s stock market ticker symbol 🍎

---

## Summary

> SQL is not just a tool — it is a career skill that opens doors everywhere.
> Pandas and SQL are partners, not competitors — SQL handles the heavy lifting, pandas handles the analysis.

