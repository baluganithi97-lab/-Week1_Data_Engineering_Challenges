# 📘 Week 1 – Data Engineering Challenges

Each task demonstrates a key data engineering skill, from simple data reading to big data processing and data warehouse design.

---

## 🟢 EASY LEVEL

### 1️⃣ Write a Python script to parse a CSV file

**🧠 Explanation:**  
A CSV (Comma-Separated Values) file stores tabular data.  
This script reads a CSV file using **Pandas**, displays the first few rows, and prints the column names.  

**💻 Code:**
```python
# parse_csv.py
# Purpose: Read and inspect data from a CSV file.

import pandas as pd

# Read CSV file
df = pd.read_csv("data.csv")

# Display first 5 rows
print(df.head())

# Print column names
print("Columns:", df.columns.tolist())
```

---

### 2️⃣ Create a SQL query to select all rows from a table

**🧠 Explanation:**  
A simple SQL command to fetch all records from a table named `employees`.  
This is a basic operation used in almost every database query.

**💻 Code:**
```sql
-- select_all_rows.sql
-- Purpose: Retrieve all rows from a table.

SELECT * FROM employees;
```

---

## 🟡 INTERMEDIATE LEVEL

### 3️⃣ Build an ETL script that loads JSON data into PostgreSQL

**🧠 Explanation:**  
ETL (Extract, Transform, Load) is a key data engineering process.  
This script reads data from a JSON file and loads it into a PostgreSQL table using the `psycopg2` library.

**💻 Code:**
```python
# etl_json_to_postgres.py
# Purpose: Load data from JSON into PostgreSQL (ETL Process).

import json
import psycopg2

# Extract: Load JSON data
with open('data.json') as f:
    data = json.load(f)

# Connect to PostgreSQL
conn = psycopg2.connect(
    host="localhost",
    database="mydb",
    user="postgres",
    password="yourpassword"
)
cursor = conn.cursor()

# Create table if not exists
cursor.execute("""
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    age INT,
    city VARCHAR(100)
)
""")

# Load data into table
for record in data:
    cursor.execute(
        "INSERT INTO users (name, age, city) VALUES (%s, %s, %s)",
        (record['name'], record['age'], record['city'])
    )

conn.commit()
cursor.close()
conn.close()
```

---

### 4️⃣ Write a SQL query with a JOIN and GROUP BY

**🧠 Explanation:**  
We use a **JOIN** to combine data from multiple tables and a **GROUP BY** to aggregate results.  
For example, counting the number of employees per department.

**💻 Code:**
```sql
-- join_groupby.sql
-- Purpose: Combine tables and summarize data using JOIN and GROUP BY.

SELECT d.department_name, COUNT(e.employee_id) AS total_employees
FROM employees e
JOIN departments d
ON e.department_id = d.department_id
GROUP BY d.department_name;
```

---

## 🔴 HARD LEVEL

### 5️⃣ Implement a PySpark job to count word frequency from text files

**🧠 Explanation:**  
This task uses **PySpark**, a distributed computing framework, to count how often each word appears in a text file.  
It’s a typical “word count” program that demonstrates Big Data processing.

**💻 Code:**
```python
# pyspark_wordcount.py
# Purpose: Count word frequencies from a text file using PySpark.

from pyspark.sql import SparkSession

# Create Spark session
spark = SparkSession.builder.appName("WordCount").getOrCreate()

# Read text file
rdd = spark.sparkContext.textFile("data.txt")

# Split lines into words and count
word_counts = (rdd.flatMap(lambda line: line.split(" "))
                   .map(lambda word: (word, 1))
                   .reduceByKey(lambda a, b: a + b))

# Display results
for word, count in word_counts.collect():
    print(f"{word}: {count}")

spark.stop()
```

---

### 6️⃣ Design a Star Schema for an E-commerce Dataset

**🧠 Explanation:**  
A **Star Schema** is a database model used in data warehousing.  
It has one **Fact Table** (main transactional data) and several **Dimension Tables** (descriptive information).  
This helps perform fast analytical queries.

**💻 Code:**
```sql
-- star_schema_ecommerce.sql
-- Purpose: Define a Star Schema for an e-commerce analytics database.

-- Dimension Tables
CREATE TABLE dim_customer (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    location VARCHAR(100)
);

CREATE TABLE dim_product (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2)
);

CREATE TABLE dim_date (
    date_id SERIAL PRIMARY KEY,
    date DATE,
    month INT,
    year INT
);

-- Fact Table
CREATE TABLE fact_orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES dim_customer(customer_id),
    product_id INT REFERENCES dim_product(product_id),
    date_id INT REFERENCES dim_date(date_id),
    quantity INT,
    total_amount DECIMAL(10,2)
);
```

---

## ✅ Summary

| Level | Task | Concept | Technology |
|:------:|------|----------|-------------|
| Easy | Parse CSV | File reading | Python (Pandas) |
| Easy | Select All Rows | Data retrieval | SQL |
| Intermediate | JSON → PostgreSQL | ETL Process | Python + PostgreSQL |
| Intermediate | JOIN + GROUP BY | Data aggregation | SQL |
| Hard | Word Frequency Count | Distributed processing | PySpark |
| Hard | Star Schema | Data Modeling | SQL / Data Warehouse |

---
