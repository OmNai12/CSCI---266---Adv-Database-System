# Battleships Relational Database (Neon + PostgreSQL + Python)

This project demonstrates how to create and query a **relational database of battleships** using **PostgreSQL** hosted on **Neon**.  
The code is written in Python and designed to run in **Google Colab**.

---

## 🚀 Features
- Creates tables for:
  - `Classes` (ship classes, guns, displacement, etc.)
  - `Ships` (individual ships and launch years)
  - `Battles` (historic naval battles)
  - `Outcomes` (ships and their results in battles)
- Inserts **sample data** from *Fighting Ships of World War II*.
- Runs SQL queries using `psycopg2` and displays results with `pandas`.

---

## 📂 Notebook Structure
The notebook (`Untitled1.ipynb`) is divided into cells:

1. **Setup**
   - Install dependencies: `psycopg2-binary`, `pandas`, `python-dotenv`
   - Import libraries and connect to Neon using credentials from `.env`

2. **Schema & Data**
   - Create tables (`Classes`, `Ships`, `Battles`, `Outcomes`)
   - Insert sample data

3. **Queries**
   - **Query 1:** Find all ships launched before 1921  
     ```sql
     SELECT name, launched FROM Ships WHERE launched < 1921;
     ```
   - **Query 2:** List ships with their country and displacement  
     ```sql
     SELECT s.name, c.country, c.displacement
     FROM Ships s
     JOIN Classes c ON s.class = c.class;
     ```
   - **Query 3:** List ships engaged in the Battle of Guadalcanal with displacement and number of guns  
     ```sql
     SELECT s.name, c.displacement, c.numGuns
     FROM Outcomes o
     JOIN Ships s ON o.ship = s.name
     JOIN Classes c ON s.class = c.class
     JOIN Battles b ON o.battle = b.name
     WHERE b.name = 'Guadalcanal';
     ```

4. **Close Connection**
   - Closes the PostgreSQL connection

---

## 🔧 Requirements
- Python 3.9+
- PostgreSQL (via [Neon](https://neon.tech/))
- Dependencies:
  ```bash
  pip install psycopg2-binary pandas python-dotenv
  ```

---

## 🔑 Environment Variables
The notebook loads credentials from a `.env` file. Example:

```env
PGDATABASE=neondb
PGUSER=your_username
PGPASSWORD=your_password
PGHOST=your_neon_host
PGPORT=5432
PGSSLMODE=require
```

---

## ▶️ Running in Google Colab
1. Upload `.env` to your Colab session (`Files → Upload`).
2. Open the notebook in Colab.
3. Run cells in order:
   - Setup
   - Schema & Data
   - Queries
   - Close Connection

---

## 📊 Example Output
- **Ships launched before 1921**  
  Haruna (1915), Hiei (1914), Kongo (1913), Kirishima (1915), …  
- **Ships with country and displacement**  
  California → USA → 32000  
  Haruna → Japan → 32000  
  …  
- **Ships in Battle of Guadalcanal**  
  Kirishima → 32000 tons → 8 guns  
  South Dakota → 46000 tons → 9 guns  
  Washington → 37000 tons → 9 guns  
