# 📊 Databricks Medallion Architecture – Retail Data Pipeline  
### **Bronze → Silver → Gold | Delta Lake | Unity Catalog | Workflows | GitHub Integration**

## 🚀 **Project Overview**  
This project implements a **complete end-to-end data engineering pipeline** using **Databricks Free Edition**, following the **Medallion Architecture** standards widely used in enterprise data platforms.

It processes two datasets:

- `retail_dataset.csv`
- `customer_dataset.json`

Through the layers:

- **Bronze** → Raw ingestion  
- **Silver** → Data cleaning & transformation  
- **Gold** → Business-ready analytical tables  

Additional features:

- ✔ Automations using **Databricks Workflows**  
- ✔ GitHub integration using **Repos (Git folders)**  
- ✔ Scheduled job runs  
- ✔ Email notifications  
- ✔ Delta Lake ACID storage  

---

## 🏛 **Architecture Diagram**

```
                RAW SOURCE FILES
     (retail_dataset.csv, customer_dataset.json)
                      │
                      ▼
      🟫 BRONZE LAYER — RAW INGESTION
      - Read from UC Volumes
      - Add ingestion metadata
      - Save as Delta
                      │
                      ▼
      ⚪ SILVER LAYER — CLEANED DATA
      - Fix date formats
      - Clean price, age, gender, city
      - Remove duplicates
      - Add totalAmount
      - Normalize values
                      │
                      ▼
      🟡 GOLD LAYER — BUSINESS TABLES
      - Customer sales summary
      - Product performance
      - City revenue metrics
      - Loyalty tier analytics
      - Monthly revenue KPIs
                      │
                      ▼
        📊 POWER BI / DATABRICKS SQL DASHBOARDS
```

---

## 🧱 **Medallion Layers Explained**

### 🟫 **Bronze Layer — Raw Ingestion**  
Location:  
`/Volumes/project4cat/project4db/p4bronze/`

Operations:

- Reads raw CSV/JSON from Unity Catalog Volumes  
- Uses inferSchema for dynamic ingestion  
- Adds metadata:  
  - `ingestion_timestamp`  
  - `source_file`  
- No cleaning — direct raw → Delta conversion  

Outputs:  
- `retail_bronze`  
- `customer_bronze`

---

### ⚪ **Silver Layer — Clean & Standardize**  
Location:  
`/Volumes/project4cat/project4db/p4silver/`

#### 📌 Customer Data Transformations:
- Standardize gender → *Male / Female / Unknown*
- Clean city: trim, uppercase, null → “UNKNOWN”
- Age cleanup: negative/invalid → null
- Loyalty tier normalization → uppercase
- Multi-format date parsing for `signup_date`
- Remove duplicates
- Schema enforcement

#### 📌 Retail Data Transformations:
- Clean price: remove “$”, cast to double
- Parse multiple `order_date` formats
- Standardize payment type & order status
- Convert returned → boolean
- Add `totalAmount = price × quantity`
- Remove duplicates

Outputs:  
- `customer_silver`  
- `retail_silver`

---

### 🟡 **Gold Layer — Analytics & Aggregations**  
Location:  
`/Volumes/project4cat/project4db/p4gold/`

Gold tables include:

### ✔ `sales_gold`  
Enriched fact table (retail + customer join)

### ✔ `customer_sales_summary`
- Total spend  
- Total orders  
- LTV metrics  
- City, gender, loyalty tier breakdowns  

### ✔ `product_sales_summary`
- Units sold  
- Total revenue  
- Order count  

### ✔ `city_sales_summary`
- City-level KPIs  
- Revenue by region  

### ✔ `loyalty_sales_summary`
- Performance of loyalty groups  

### ✔ `monthly_revenue`
- Year-month revenue trend  

---

## ⚙️ **Orchestration Using Databricks Workflows**

Pipeline Tasks:

```
bronze_task → silver_task → gold_task
```

Features:

- ✔ Scheduled every 10 minutes (demo mode)  
- ✔ Email alerts on success/failure  
- ✔ Each task runs independently  
- ✔ Automatic end-to-end execution  

---

## 🔄 **GitHub Integration (Databricks Repos)**

This project is fully synced with GitHub using:

- Personal Access Token (PAT)  
- Databricks Repos (Git folder)  
- Commit & Sync from Databricks UI  

You can push notebook changes instantly:

```
Git → Commit & Sync → Push
```

---

## 📁 **Repository Structure**

```
databricks-medallion-retail-pipeline/
│
├── Project4/Notebooks/
│   ├── 01_project4_BronzeLayer.ipynb
│   ├── 02_project4_SilverLayer.ipynb
│   ├── 03_project4_GoldLayer.ipynb
│   ├── Project4_Setup.ipynb
│
├── data/
│   ├── retail_dataset.csv
│   ├── customer_dataset.json
│
├── architecture/
│   ├── medallion_architecture.png
│   ├── workflow_diagram.png
│
├── docs/
│   ├── pipeline_overview.md
│   ├── gold_tables_description.md
│   ├── business_requirements.md
│
└── README.md
```

---

## 📈 **Example Queries for Dashboards**

### Monthly Revenue
```sql
SELECT month, monthly_revenue 
FROM p4gold.monthly_revenue 
ORDER BY month;
```

### City Revenue
```sql
SELECT city, total_revenue 
FROM p4gold.city_sales_summary 
ORDER BY total_revenue DESC;
```

### Customer LTV
```sql
SELECT customer_id, total_spend, total_orders 
FROM p4gold.customer_sales_summary 
ORDER BY total_spend DESC;
```

---

## 🧪 **How to Reproduce**

### 1️⃣ Upload raw data  
Place files into UC Volumes:

```
/Volumes/project4cat/project4db/p4raw/
```

### 2️⃣ Run Notebooks (in order):
1. **01 Bronze**  
2. **02 Silver**  
3. **03 Gold**

### 3️⃣ Setup Databricks Workflow  
- Add tasks (Bronze → Silver → Gold)  
- Schedule (every 10 min or daily)  
- Add email notifications  

---

## 🎯 **Skills Demonstrated**

- Databricks Unity Catalog  
- Delta Lake (ACID)  
- PySpark transformations  
- GitHub integration  
- Workflow automation  
- Medallion Architecture  
- Data modeling (Fact/Dimension)  
- Dashboard-ready Gold tables  

---

## 👨‍💼 **Author**  
**Tejas Gawali**  
Azure Data Engineer | Databricks | PySpark | Delta Lake | SQL  
GitHub: https://github.com/tejas13gawali
