# 🛒 E-commerce Customer Churn Analysis

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Hadoop](https://img.shields.io/badge/Hadoop-3.4.3-yellow?logo=apache)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)

---

## 📌 Project Overview

This project analyzes an E-commerce Customer Churn dataset containing **50,000 customer records** and **25 features**. The goal is to explore behavioral patterns that lead to customer churn using Big Data tools including **Docker**, **Hadoop (HDFS + MapReduce)**, and **Python**.

---

## 📁 Project Structure

```
📦 ecommerce-churn-bigdata/
├── 📂 data/
│   ├── ecommerce_customer_churn_dataset.csv   # Original dataset
│   └── churn_cleaned.csv                      # Cleaned dataset
├── 📂 notebooks/
│   └── churn_analysis.ipynb                   # Full Jupyter Notebook
├── 📂 hadoop/
│   ├── mapper.py                              # MapReduce Mapper
│   └── reducer.py                             # MapReduce Reducer
├── 📂 charts/
│   ├── bar_chart.png                          # Churn by Country
│   ├── pie_chart.png                          # Churn Distribution
│   ├── line_chart.png                         # Lifetime Value by Age
│   └── histogram.png                          # Session Duration
├── 📂 report/
│   └── Big_Data_Project_Report.md             # Full Project Report
└── README.md
```

---

## 🗂️ Dataset

| Field | Details |
|---|---|
| Source | [Kaggle — E-commerce Customer Churn Dataset](https://www.kaggle.com) |
| Format | CSV |
| Records | 50,000 customers |
| Features | 25 columns |
| Target | `Churned` (1 = Left, 0 = Stayed) |
| Churn Rate | ~29% |

### Key Columns
| Column | Description |
|---|---|
| `Age` | Customer age |
| `Country` / `City` | Location |
| `Total_Purchases` | Number of purchases |
| `Cart_Abandonment_Rate` | % of carts abandoned |
| `Lifetime_Value` | Total value to business ($) |
| `Session_Duration_Avg` | Avg session duration (min) |
| `Churned` | **Target variable** |

---

## ⚙️ Environment Setup

### Requirements
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- No local Python or Hadoop installation needed — everything runs inside Docker

### Docker Containers Used

| Container | Image | Purpose |
|---|---|---|
| `hadoop-container` | `farberg/apache-hadoop` | Hadoop 3.4.3 (HDFS + MapReduce) |
| `jupyter-nb` | `continuumio/anaconda3` | Jupyter Notebook + Python |

### Pull & Run Containers

```bash
# 1. Hadoop Container
docker pull farberg/apache-hadoop
docker run -it --name hadoop-container farberg/apache-hadoop bash

# 2. Jupyter Container
docker pull continuumio/anaconda3
docker run -p 8888:8888 \
  -v "C:/Users/<YourName>/Downloads:/home/jovyan/work" \
  --name jupyter-nb \
  continuumio/anaconda3 \
  jupyter notebook --ip=0.0.0.0 --allow-root --no-browser
```

Then open Jupyter at: `http://127.0.0.1:8888`

---

## 🧹 Part 1: Data Preprocessing

All preprocessing done inside `churn_analysis.ipynb`

| Step | Action | Result |
|---|---|---|
| Load | Read CSV with pandas | 50,000 rows × 25 cols |
| Understand | `df.info()` + `df.describe()` | 23 numeric, 2 string cols |
| Duplicates | `drop_duplicates()` | 0 duplicates found ✅ |
| Missing Values | Fill with column median | 12+ columns fixed ✅ |
| Inconsistencies | Standardize Gender, Country | Text cleaned ✅ |
| Data Types | Convert Age, Purchases to int | Types fixed ✅ |

```python
# Quick preprocessing snippet
import pandas as pd
import numpy as np

df = pd.read_csv('work/ecommerce_customer_churn_dataset.csv')
df.drop_duplicates(inplace=True)

numeric_cols = df.select_dtypes(include=np.number).columns
df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].median())

df['Gender'] = df['Gender'].str.strip().str.capitalize()
df['Country'] = df['Country'].str.strip().str.title()

df['Age'] = df['Age'].astype(int)
df['Total_Purchases'] = df['Total_Purchases'].astype(int)
df['Churned'] = df['Churned'].astype(int)

df.to_csv('work/churn_cleaned.csv', index=False)
```

---

## 🐘 Part 2: Hadoop — HDFS + MapReduce

### HDFS Storage

```bash
# Copy CSV into Hadoop container
docker cp "ecommerce_customer_churn_dataset.csv" hadoop-container:/root/churn.csv

# Upload to HDFS
hdfs dfs -mkdir -p /user/churn
hdfs dfs -put /root/churn.csv /user/churn/
hdfs dfs -ls /user/churn/
```

**Result:**
```
Found 1 items
-rw-r--r--  1 root root  6112281  2026-05-07  /user/churn/churn.csv
```

### MapReduce — Churn Count by Country

**mapper.py** — Emits `(country, 1)` for each churned customer
```python
#!/usr/bin/env python3
import sys

for line in sys.stdin:
    line = line.strip()
    if line.startswith('Age'):
        continue
    cols = line.split(',')
    try:
        country = cols[2]
        churned = cols[23]
        if churned == '1':
            print(f"{country}\t1")
    except:
        pass
```

**reducer.py** — Sums churn count per country
```python
#!/usr/bin/env python3
import sys

current_country = None
current_count = 0

for line in sys.stdin:
    line = line.strip()
    country, count = line.split('\t')
    count = int(count)
    if current_country == country:
        current_count += count
    else:
        if current_country:
            print(f"{current_country}\t{current_count}")
        current_country = country
        current_count = count

if current_country:
    print(f"{current_country}\t{current_count}")
```

**Run the job:**
```bash
hadoop jar /opt/hadoop-3.4.3/share/hadoop/tools/lib/hadoop-streaming-*.jar \
  -input /user/churn/churn.csv \
  -output /user/churn/output \
  -mapper /root/mapper.py \
  -reducer /root/reducer.py

# View results
hdfs dfs -cat /user/churn/output/part-00000
```

---

## 📊 Part 3: Visualizations

Four charts generated using Matplotlib & Seaborn:

| Chart | Type | Insight |
|---|---|---|
| `bar_chart.png` | Bar Chart | Top 8 countries by churn count |
| `pie_chart.png` | Pie Chart | 71% stayed vs 29% churned |
| `line_chart.png` | Line Chart | Lifetime value increases with age |
| `histogram.png` | Histogram | Session duration distribution |

---

## 💡 Key Insights

- **29% churn rate** — nearly 1 in 3 customers left the platform
- **High cart abandonment** is strongly linked to churn
- **Older customers** have higher lifetime value and lower churn tendency
- **Short session durations** indicate disengagement and higher churn risk
- **Geographic patterns** — certain countries show significantly higher churn

---

## ⚠️ Challenges & Solutions

| Challenge | Solution |
|---|---|
| Old Hadoop images incompatible with Docker | Used `farberg/apache-hadoop` instead |
| `scipy-notebook` read-only filesystem error | Switched to `continuumio/anaconda3` |
| Port 8888 already in use | Used port `8889` as alternative |
| `python3` missing in Hadoop container | Ran `apt-get install -y python3` |
| Missing values in 12+ columns | Applied median imputation |

---
