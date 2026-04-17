# 🌍 North Africa M&E — Maternal Mortality Predictive Analysis

> **Machine Learning for Monitoring & Evaluation** | Predictive modelling of maternal mortality across five North African countries (Algeria, Egypt, Libya, Morocco, Tunisia) for the period 2000–2023.

---

## 📋 Project Overview

This project applies **Andrew NG's Machine Learning Diagnostic Framework** to a real-world M&E dataset combining FAOSTAT, World Bank WGI, and WHO indicators. The goal is to identify the structural drivers of maternal mortality in North Africa, build a generalising predictive model, segment countries into actionable policy clusters, and project trajectories to 2030 against the SDG 3.1 target (≤70 deaths per 100,000 live births).

| Dimension | Detail |
|---|---|
| **Countries** | Algeria, Egypt, Libya, Morocco, Tunisia |
| **Period** | 2000–2023 |
| **Target variable** | Maternal Mortality Rate (per 100,000 live births) |
| **Data sources** | FAOSTAT · World Bank WGI · WHO |
| **ML Framework** | Andrew NG's Train/CV/Test bias-variance diagnostic |
| **Best model** | Polynomial Regression (deg=2 + Ridge), R²_cv = 0.985 |

---

## 📁 Repository Structure

```
Health_Governance_NorthAfrica/
│
├── data/
│   ├── faostat_food_security.csv              # FAOSTAT raw extract — food security indicators
│   ├── worldbank_governance.csv               # World Bank WGI raw extract — governance scores
│   ├── who_health_indicators.csv              # WHO raw extract — health indicators
│   └── north_africa_final_clean.csv           # Final merged & cleaned dataset (113 rows × 12 columns)
│
├── notebooks/
│   ├── Data_Cleaning_FAO_WB_WHO.ipynb         # NOTEBOOK 1 — Data cleaning & preparation
│   ├── EDA_FAO_WB_WHO.ipynb                   # NOTEBOOK 2 — Exploratory data analysis
│   └── ML_FAO_WB_WHO_v5.ipynb                 # NOTEBOOK 3 — ML pipeline (final version)
│
├── outputs/
│   ├── NorthAfrica_ME_Dashboard.html          # Interactive 4-page HTML dashboard
│   ├── NorthAfrica_ME_PowerBI.xlsx            # Power BI data package (10 tabs + DAX measures)
│   └── NorthAfrica_ME_KeyFindings.md          # Key findings & policy recommendations
│
└── README.md
```

---

## 🔬 Methodology

### 1. Data Collection & Cleaning (`Data_Cleaning_FAO_WB_WHO.ipynb`)

Three independent raw datasets were collected and merged:

**Dataset 1 — FAOSTAT (Food Security and Nutrition)**
Source: https://www.fao.org/faostat/en/#data

Three indicators extracted for 5 North African countries over 2000–2023:
- `Undernourishment_pct` — Prevalence of undernourishment (3-year average, %)
- `FoodSupply_variability` — Per capita food supply variability (kcal/capita/day)
- `SafeWater_pct` — Population using safely managed drinking water services (%)

Key cleaning steps applied to the FAO dataset:
- FAO data uses 3-year interval labels (e.g. "2000–2002") → converted to the **middle year** (2001) to align with annual World Bank and WHO data
- Column renaming and pivot to wide format (one indicator per column)
- Missing value imputation (see table below)

**Dataset 2 — World Bank Governance Indicators (WGI)**
Source: https://databank.worldbank.org/source/worldwide-governance-indicators

Four governance score indicators (0–100 scale) retained; standard error and confidence interval columns excluded. Country name harmonised ("Egypt, Arab Rep." → "Egypt"). Columns reshaped from wide (years as columns) to long format via `melt()`, then pivoted to wide indicator format.

Indicators extracted:
- `Gov_Effectiveness` · `Rule_of_Law` · `Control_Corruption` · `Voice_Accountability`

**Dataset 3 — WHO Global Health Observatory**
Source: https://www.who.int/data/gho/data/indicators

Three health indicators extracted and renamed:
- `Life_Expectancy` · `Health_Expenditure_GDP` · `Maternal_Mortality`

**Merge strategy:** Sequential inner join on `[Country, Year]`: FAO ⋈ World Bank → FAO+WB ⋈ WHO. Final dataset: **113 rows × 12 columns**.

**Missing value imputation log:**

| Variable | Countries | Method | Rationale |
|---|---|---|---|
| `Undernourishment_pct` | All 5 | Linear interpolation by country | Gradual temporal trend — interpolation preserves trajectory |
| `SafeWater_pct` | Tunisia, Algeria, Morocco | Linear interpolation by country | Gradual trend — interpolation appropriate |
| `SafeWater_pct` | **Egypt, Libya** | **No imputation — NaN retained** | FAO data not available — absence is informative; imputation would introduce bias |
| `FoodSupply_variability` | All 5 | Median by country | Volatile indicator — median robust to extreme values |

> ⚠️ **Methodological note:** Safe water access data is entirely absent for Egypt and Libya for 2002–2023. These values are intentionally preserved as `NaN`. Analyses involving this indicator are therefore limited to Tunisia, Algeria, and Morocco. Following standard M&E practice: *"The absence of data is itself information."*

**Final column order:**
```
Country | Year | Undernourishment_pct | FoodSupply_variability | SafeWater_pct |
Life_Expectancy | Health_Expenditure_GDP | Maternal_Mortality |
Gov_Effectiveness | Rule_of_Law | Control_Corruption | Voice_Accountability
```

---

### 2. Exploratory Data Analysis (`EDA_FAO_WB_WHO.ipynb`)

- Distribution and trend analysis per indicator and per country
- Pearson correlation matrix — identification of key bivariate relationships
- Key finding: `SafeWater_pct` has the strongest correlation with `Maternal_Mortality` (r = −0.915)
- Correlation between `Health_Expenditure_GDP` and `Maternal_Mortality` significant only for Morocco (r = −0.88)
- Life expectancy gain analysis: Morocco +8.5 years (66.8 → 75.3) — largest in the region

---

### 3. Multicollinearity Analysis & Feature Selection (`ML_FAO_WB_WHO_v5.ipynb`)

- Pearson correlation heatmap identified **5 high-collinearity pairs** (|r| > 0.7)
- Three feature sets tested: **A** (all 8 features), **C** (5 low-VIF), **D** (C + FoodSupply_variability)
- **Feature Set A retained** for the final model — polynomial and ridge models handle correlated features effectively without performance loss

---

### 4. Model Selection — Andrew NG's Bias-Variance Framework

- **Split:** 60% Train / 20% CV / 20% Test (`random_state=42`)
- **Primary overfitting signal:** MSE ratio = J_cv / J_train
- **5 models compared** across 3 feature sets

| Model | R²_train | R²_cv | J_cv / J_train | Diagnosis |
|---|---|---|---|---|
| Linear Regression | 0.946 | 0.842 | 1.5× | Moderate Variance |
| Ridge Regression | 0.946 | 0.862 | 1.3× | Moderate Variance |
| **Poly+Ridge (deg=2)** | **0.997** | **0.985** | **3×** | **✅ Selected** |
| Random Forest | 0.994 | 0.934 | 5× | HIGH VARIANCE |
| Gradient Boosting | 1.000 | 0.936 | 222× | HIGH VARIANCE |

**Selected model:** Polynomial Regression (degree=2) + Ridge regularisation, Feature Set A.

**Bias vs Variance — definitive distinction (Andrew NG):**
- **High Bias (Underfitting):** Both J_train and J_cv plateau at a high error level regardless of training size. More data does **not** help. Remedy: more complex model or polynomial features.
- **High Variance (Overfitting):** J_train is low but J_cv >> J_train. More data **can** help. Remedy: regularisation (Ridge/Lasso) or reduce model complexity.

Tree-based models (GB ratio 222×, RF ratio 5×) overfit severely on n=69. Ridge regularisation is the correct remedy, not feature removal.

---

### 5. K-Means Clustering

- **K=3** selected — M&E business justification over statistical optimum (silhouette peaks at K=5–6)
- 50 random restarts (`n_init=50`, k-means++) to avoid local minima
- Three actionable policy segments:

| Cluster | Label | Countries | M&E Purpose |
|---|---|---|---|
| 0 | **High Performers** | Algeria, Tunisia | Reference benchmark for policy transfer |
| 1 | **Mid-level Developers** | Egypt, Morocco | Priority group for targeted interventions |
| 2 | **Fragile / Post-conflict** | Libya | Conflict-sensitive programming |

---

### 6. Projections (2024–2030)

Three extrapolation models fitted per country: Linear, Polynomial (deg=2), Logarithmic.

| Country | Recommended model | 2030 estimate | SDG 3.1 (≤70) |
|---|---|---|---|
| Morocco | **Logarithmic** | 25.7/100k | ✅ Achieved |
| Egypt | Linear | 12.1/100k | ✅ Achieved |
| Algeria | Linear | 58.4/100k | ✅ Achieved |
| Tunisia | Linear | 34.9/100k | ✅ Achieved |
| **Libya** | **Linear (flat)** | **73.4/100k** | **⚠️ At risk** |

> Morocco: Linear model projects values ≤ 0 by 2028 (floor artefact). Logarithmic model captures diminishing returns — epidemiologically correct as mortality approaches a biological floor.
> Libya: R² ≈ 0.02 — no reliable trend. Standard projections are not meaningful in a conflict context.

---

## 📊 Key Findings

1. **SafeWater_pct** is the dominant structural predictor of maternal mortality (Pearson r = −0.915)
2. **Morocco** achieved the steepest improvement (−74.2%, 271→70/100k): only country where health expenditure correlates significantly with maternal mortality (r = −0.88); safe water access rose from 42% to 75%; life expectancy gained +8.5 years
3. **Libya** is the only country at risk of missing SDG 3.1 by 2030 (linear: 73.4, log: 73.3/100k)
4. **Tree-based models overfit** on small datasets (n=69) — Poly+Ridge is the superior choice
5. **FoodSupply_variability** adds no predictive value — it is a crisis indicator, not a structural predictor

---

## 📈 Dashboard

The interactive HTML dashboard (`outputs/NorthAfrica_ME_Dashboard.html`) contains 4 pages:

| Page | Content |
|---|---|
| ① Executive Overview | KPI cards · Maternal mortality trend 2000–2023 · % reduction |
| ② Indices & Clustering | Governance/Health/Food Security over time · K=3 cluster profiles · Radar chart |
| ③ ML Insights | Bias-variance diagnostic · Feature importance · SafeWater scatter plot |
| ④ Projections 2030 | Linear/Polynomial/Log projections · SDG gap (3 models) · Best model selection table |

**How to view locally:** Download `NorthAfrica_ME_Dashboard.html` and open it in any modern browser — no installation required.

**How to view on GitHub:** See the [GitHub Pages section](#-how-to-run-the-dashboard-on-github-pages) below.

---

## 🌐 How to Run the Dashboard on GitHub Pages

GitHub cannot render `.html` files directly in the browser from a standard repository view — it shows raw code instead. To make the dashboard accessible as a live webpage, use **GitHub Pages**:

**Step-by-step:**

1. Go to your repository on GitHub → **Settings** tab
2. Scroll down to **Pages** (left sidebar)
3. Under **Source**, select `Deploy from a branch`
4. Choose branch: `main` · folder: `/ (root)` → click **Save**
5. Wait ~2 minutes — GitHub will display a live URL:
   `https://asmaboufaden.github.io/real-case-studies/Health_Governance_NorthAfrica/outputs/NorthAfrica_ME_Dashboard.html`

**Alternative — nbviewer for notebooks:**
To render Jupyter notebooks directly: `https://nbviewer.org/github/asmaboufaden/real-case-studies/blob/main/Health_Governance_NorthAfrica/notebooks/ML_FAO_WB_WHO_v5.ipynb`

> ⚠️ **Note:** GitHub Pages serves files from the repository root or `docs/` folder. If your HTML file is inside a subfolder (`outputs/`), the full path must be included in the URL as shown above.

---

## 🛠️ Requirements

```bash
# Core packages
pip install pandas numpy scikit-learn scipy matplotlib seaborn openpyxl

# Optional: AutoML
pip install pycaret==3.3.2     # Stable version (NOT 3.0.0 — known dependency conflicts)
pip install lazypredict         # Lightweight alternative — no install conflicts
pip install flaml               # Fast AutoML (Microsoft Research)
```

---

## 🚀 Usage

```bash
# 1. Clone the repository
git clone https://github.com/asmaboufaden/real-case-studies.git
cd real-case-studies/Health_Governance_NorthAfrica

# 2. Run notebooks in order
jupyter notebook notebooks/Data_Cleaning_FAO_WB_WHO.ipynb   # Step 1: data prep
jupyter notebook notebooks/EDA_FAO_WB_WHO.ipynb              # Step 2: exploration
jupyter notebook notebooks/ML_FAO_WB_WHO_v5.ipynb            # Step 3: ML pipeline

# 3. View the interactive dashboard
open outputs/NorthAfrica_ME_Dashboard.html    # macOS
start outputs/NorthAfrica_ME_Dashboard.html   # Windows
```

---

## 📝 Notes on Methodology

### Transparency
Python code was developed with AI-assisted tools (Claude AI, Microsoft Copilot). All outputs, interpretations, and conclusions were verified, reviewed, and validated by the author. This reflects standard practice in modern data science workflows.

### Andrew NG Framework Reference
The bias-variance diagnostic methodology is based on the approach demonstrated in lab notebooks from Andrew NG's Machine Learning Specialization (C2W3 lab notebooks provided by the author). The principle of choosing K for K-Means based on downstream business purpose rather than elbow method alone is consistent with Andrew NG's teaching approach and the broader ML literature.

### Data Limitations
- `SafeWater_pct` is entirely missing for Egypt and Libya → Food Security Index is underestimated for these countries
- n=69 complete observations after inner merge → small sample; all models show some high variance on learning curves (expected)
- COVID-19 caused spikes in maternal mortality in 2020–2021 for all countries, reducing projection R² values

---

## 👩‍💼 Author

**Asma Boufaden**
M&E Specialist | Data Analyst
📊 FAOSTAT · World Bank WGI · WHO
🗺️ North Africa Regional Analysis

---

## 📄 License

This project is for academic and professional M&E purposes. Data sources (FAOSTAT, World Bank WGI, WHO) are publicly available under their respective open data licenses.
