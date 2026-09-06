# Volve Production Data Analysis

Data Engineering & Machine Learning assessment using the [Volve Data Village](https://www.equinor.com/energy/volve-data-sharing) open dataset (Equinor).

---

## Project Structure

```
volve-project/
├── docker-compose.yml                     # Part 1: PostgreSQL + Jupyter environment
├── notebooks/
│   └── volve_production_analysis.ipynb    # Parts 2 & 3: EDA + ML
├── outputs/
│   ├── figures/                           # Generated plots
│   └── tables/                            # CSV summaries
└── data/
    └── raw/                               # Place Volve_production_data.xlsx here (not committed)
```

---

## Setup & Running

### Prerequisites
- Docker & Docker Compose installed

### 1. Start the environment

```bash
docker compose up -d
docker compose ps       # both containers should show "healthy"
```

### 2. Add the data file

Download the Volve production Excel file from [Equinor Volve Data Village](https://www.equinor.com/energy/volve-data-sharing) and place it at:

```
data/raw/Volve_production_data.xlsx
```

### 3. Open Jupyter

Go to [http://localhost:8888](http://localhost:8888) and enter token: `password`

Open `work/volve_production_analysis.ipynb` and run all cells top to bottom.

---

## What the Notebook Covers

### Part 1 — Docker Environment
- PostgreSQL 16 + Jupyter on a custom bridge network
- Healthcheck ensures DB is ready before notebook connects
- All analysis reads from the database, not directly from files

### Part 2 — Data Management & Analysis
- Schema understanding and data type mapping
- SCREAMING_SNAKE_CASE column standardisation
- Missing value analysis (~40–60% on pressure/choke columns)
- Unit conversions: oil Sm³ → bbl, gas Sm³ → Mscf
- Water handling: negative values flagged, raw values preserved
- Production vs. injection separation
- Dual-purpose well investigation (15/9-F-5)
- Correlation heatmap, outlier boxplots, production trend plots
- Key findings: peak production 2009, water breakthrough cause of decline

### Part 3 — Machine Learning

**Task 1 — Daily Oil Forecasting**
- Chronological train/test split (no random split — time series data)
- 1-day lag features + calendar features
- Models: Linear Regression (baseline), Random Forest, HistGradientBoosting
- Best model: HistGradientBoosting — R² = 0.941, MAE = 61.5

**Task 2 (Bonus) — Monthly Clustering**
- Well-month aggregations as clustering observations
- K-Means + Hierarchical Ward clustering
- K selected via Elbow method + Silhouette Score
- Two clusters identified: Early-Life vs. Mature field regime

---

## Key Findings

| Metric | Value |
|---|---|
| Total oil produced | ~10M Sm³ (63M bbl) |
| Field life | 2007 – 2016 |
| Peak production year | 2009 |
| Production wells | 6 |
| Injection wells | 2 |
| Highest oil producer | 15/9-F-12 (4.58M Sm³) |
| Highest water producer | 15/9-F-14 |
| Best ML model R² | 0.941 (HistGradientBoosting) |

---

## Data Source

Volve Data Village — Equinor open dataset.  
The raw Excel file is not committed to this repository.
