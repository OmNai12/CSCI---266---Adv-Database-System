# 🗄️ Database Queries with SQLAlchemy & Pandas

## 📌 Overview
This project demonstrates running **SQL queries** from Python using **SQLAlchemy** and **Pandas**.  
The database contains four related tables:

- `product` – makers and models  
- `pc` – PC models and their details  
- `laptop` – Laptop models and their details  
- `printer` – Printer models and their details  

The queries explore different aspects of the dataset such as most expensive PCs, number of laptops per maker, and unified product catalogs.

---

## ⚙️ Tech Stack
- **Python 3**
- **SQLAlchemy** – database connection
- **SQLite / Any RDBMS** – database backend
- **Pandas** – data analysis & display

---

## 📊 Implemented Queries

### 🔹 Query 1 – Most Expensive PC per Maker
Finds the most expensive **PC model** for each maker.

```sql
SELECT p.maker, pc.model, pc.price
FROM product p
JOIN pc ON p.model = pc.model
WHERE pc.price = (
    SELECT MAX(pc2.price)
    FROM product p2
    JOIN pc pc2 ON p2.model = pc2.model
    WHERE p2.maker = p.maker
)
ORDER BY pc.price DESC;
```

---

### 🔹 Query 2 – Laptop Models per Maker
Counts how many **laptop models** each maker produces.

```sql
SELECT p.maker, COUNT(*) AS laptop_count
FROM product p
JOIN laptop l ON p.model = l.model
GROUP BY p.maker
ORDER BY laptop_count DESC, p.maker;
```

---

### 🔹 Query 3 – Unified Catalog with Price Ranges
Creates a combined catalog of all products (PCs, Laptops, Printers)  
and summarizes **minimum, maximum, and average price** per maker and type.

```sql
WITH all_products AS (
    SELECT p.maker, p.model, 'PC' AS type, pc.price
    FROM product p JOIN pc ON p.model = pc.model
    UNION ALL
    SELECT p.maker, p.model, 'Laptop', l.price
    FROM product p JOIN laptop l ON p.model = l.model
    UNION ALL
    SELECT p.maker, p.model, 'Printer', pr.price
    FROM product p JOIN printer pr ON p.model = pr.model
)
SELECT maker, type, 
       MIN(price) AS min_price, 
       MAX(price) AS max_price, 
       ROUND(AVG(price),2) AS avg_price
FROM all_products
GROUP BY maker, type
ORDER BY maker, type;
```

---

## 🚀 How to Run

Each query is executed in Python and results are displayed in Pandas DataFrames.  

Example:
```python
with engine.connect() as conn:
    q1 = """
    SELECT p.maker, pc.model, pc.price
    FROM product p
    JOIN pc ON p.model = pc.model
    WHERE pc.price = (
        SELECT MAX(pc2.price)
        FROM product p2
        JOIN pc pc2 ON p2.model = pc2.model
        WHERE p2.maker = p.maker
    )
    ORDER BY pc.price DESC;
    """
    q1_df = pd.read_sql_query(text(q1), conn)
q1_df
```

---

## 📂 Project Structure
```
├── queries.ipynb     # Jupyter / Colab notebook with queries
├── README.md         # Documentation
└── requirements.txt  # Dependencies (optional)
```

---

## 📌 Notes
- The schema used here is the classic **Product-PC-Laptop-Printer** schema used in database courses.  
- Queries are kept **modular** and can be extended easily.  
- Works with any SQLAlchemy-compatible database (`SQLite`, `Postgres`, `MySQL`, etc.).  

---

✨ With this setup, you can explore the database, run queries, and analyze results directly in Python!  
