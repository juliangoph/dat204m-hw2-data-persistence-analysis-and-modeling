# DAT204M HW2 — Cloud Data Pipeline and Analysis (Track A)

### 📘 Course
**DAT204M – Data Collection and Processing**  
De La Salle University  
Term 1 AY 2025–2026  

---

## 📖 Project Overview

This project demonstrates a complete data pipeline from **local CSV → cloud database → analysis notebook**.

It builds directly on **HW1 (data collection and cleaning)** and fulfills the HW2 objectives:

1. Persist clean data into a **cloud database** (Supabase).  
2. Query data **directly from the cloud** for exploratory data analysis (EDA) and modeling.  
3. Apply either a **simple machine learning model** or an **in-depth statistical analysis**.  
4. Document the workflow, database schema, and results in professional, reproducible notebooks.

---

## ⚙️ Architecture

```
🗄️ 00_data_collection.ipynb
     │
     ▼
📄 CSV (clean dataset)
     │
     ▼
🧩 01_upload_to_db.ipynb
     ├── reads local CSV
     ├── sanitizes NaN / inf → None (JSON-safe)
     ├── creates table schema (if needed)
     ├── uploads to Supabase via REST API
     ▼
☁️ Supabase Cloud Database (PostgreSQL)
     │
     ▼
📊 02_analysis_from_db.ipynb
     ├── queries data via Supabase client
     ├── performs EDA & visualizations
     ├── trains Linear Regression model
     ├── evaluates & interprets results
```

---

## 🧰 Files Included

| File | Description |
|------|--------------|
| `00_data_collection.ipynb` | Collects data from World Bank Open Data. |
| `01_upload_to_db.ipynb` | Uploads the cleaned CSV to Supabase securely. |
| `02_analysis_from_db.ipynb` | Queries from Supabase, performs EDA & ML modeling. |
| `requirements.txt` | List of Python dependencies (see below). |
| `.env` *(not committed)* | Environment variables for Supabase credentials. |
| `README.md` | This documentation file. |
| `data/` | Folder containing your cleaned CSV file. |

---

## 🗝️ Environment Setup

### 1️⃣ Prerequisites
- Python ≥ 3.9  
- Jupyter Notebook or JupyterLab  
- Supabase account (free tier)

### 2️⃣ Clone and install
```bash
git clone <your-repo-url>
cd <your-repo-folder>
pip install -r requirements.txt
```

### 3️⃣ Create a `.env` file
```bash
SUPABASE_URL=https://<PROJECT_REF>.supabase.co
SUPABASE_KEY=<your-anon-key>
```

---

## 🚀 Notebook 1: Data Persistence (`01_upload_to_db.ipynb`)

**Purpose:** move the cleaned CSV into Supabase.

### Key steps
1. Read the local cleaned CSV.  
2. Sanitize all missing values (`NaN`, `inf`) → `None`.  
3. Generate a matching `CREATE TABLE` SQL (for Supabase SQL editor).  
4. Batch upload using `supabase-py`’s `upsert()` method.  
5. Verify with `maybe_single()` query and record count.

### Outputs
- Cloud table: `brn_indicators`  
- Safe, reproducible upload process with secure credentials.

---

## 📈 Notebook 2: Analysis from DB (`02_analysis_from_db.ipynb`)

**Purpose:** query data directly from Supabase for Track A (Simple ML Model).

### Key steps
1. Connect via `supabase-py` client.  
2. Query all rows from the `brn_indicators` table.  
3. Perform EDA:
   - Summary statistics  
   - Correlation heatmap & pairplots  
   - Time-series for sample country  
4. Model:
   - Train/test split  
   - Linear Regression (`scikit-learn`)  
   - Evaluate R² and RMSE  
5. Visualize feature importance & prediction diagnostics.

### Example results
- **R² ≈ 0.70–0.90** depending on dataset.  
- Strong positive relationship between energy use per capita and CO₂ emissions.  
- Renewable energy share shows slight negative effect.

---

## 🧮 Dependencies

See [`requirements.txt`](requirements.txt) for full list.  
Main packages:

| Category | Packages |
|-----------|-----------|
| Core data | `pandas`, `numpy` |
| Visualization | `matplotlib`, `seaborn` |
| ML Modeling | `scikit-learn` |
| Cloud Database | `supabase`, `postgrest`, `sqlalchemy`, `psycopg2-binary` |
| Environment Mgmt | `python-dotenv` |

Install all:
```bash
pip install -r requirements.txt
```

---

## 🧩 Database Schema Example

```sql
create table if not exists brn_indicators (
  country text,
  year int,
  co2_per_capita_tco2e_excl_lulucf double precision,
  co2_total_mtco2e_excl_lulucf double precision,
  energy_use_kg_oe_per_capita double precision,
  gdp_current_usd double precision,
  population_total double precision,
  renewable_electricity_pct double precision,
  renewable_energy_consumption_pct double precision,
  urban_pop_pct double precision,
  missing_indicator_count int,
  primary key (country, year)
);
```

---

## 🔒 Security Notes

- Store credentials only in `.env`.  
- Never expose the **Service Role Key** publicly.  
- All uploads use HTTPS REST calls (`supabase-py`), no direct DB ports.  
- RLS (Row Level Security) can be configured later if needed.

---

## 🏁 Deliverables Summary

| Deliverable | Description | Status |
|--------------|-------------|--------|
| ✅ Notebook 1 | CSV → Supabase Pipeline | Complete |
| ✅ Notebook 2 | Supabase → EDA + ML | Complete |
| ✅ requirements.txt | Dependencies | Included |
| ✅ README.md | Documentation | Included |

---

## 🧠 Learning Reflections

- Implemented real-world data pipeline using a modern cloud DB.  
- Learned secure handling of environment variables and credentials.  
- Practiced end-to-end reproducibility (data → model).  
- Applied linear regression and interpreted coefficients.  
- Reinforced principles of reusability and modular design.

---

## 📚 References
- [Supabase Python Client Docs](https://supabase.com/docs/reference/python)  
- [pandas Documentation](https://pandas.pydata.org/docs/)  
- [scikit-learn Documentation](https://scikit-learn.org/stable/)  
- [Matplotlib & Seaborn Visualization](https://seaborn.pydata.org/)  

---

© 2025 De La Salle University – Masters of Science Data Science  
Authors: Julian Roger Go, Charisse Nethercott, Gabriel Masangkay, John Carlo Gonzales, Edmar Dizon
