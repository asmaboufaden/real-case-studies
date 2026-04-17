# 📚 Real Case Studies — Applied Data Science for International Development

> A collection of applied data science and machine learning projects grounded in real-world M&E, policy analysis, and international development contexts. Each case study combines domain expertise with rigorous analytical methods to produce evidence-based, policy-relevant outputs.

---

## 🗂️ Case Studies

### 01 · [Health & Governance in North Africa](./Health_Governance_NorthAfrica/)

**Maternal Mortality Predictive Analysis — Algeria, Egypt, Libya, Morocco, Tunisia (2000–2023)**

| | |
|---|---|
| **Domain** | M&E · Global Health · Governance |
| **Data sources** | FAOSTAT · World Bank WGI · WHO |
| **Methods** | Machine Learning · K-Means Clustering · Time-Series Projections |
| **ML framework** | Andrew NG's bias-variance diagnostic (Train/CV/Test) |
| **Best model** | Polynomial Regression (deg=2 + Ridge), R²_cv = 0.985 |
| **Key finding** | Safe water access (r = −0.915) is the dominant structural predictor of maternal mortality; Morocco achieved the steepest reduction (−74.2%) driven by water infrastructure + health expenditure |
| **SDG relevance** | SDG 3.1 — Maternal Mortality ≤ 70/100k by 2030 |
| **Outputs** | Interactive HTML dashboard · Power BI data package · Key findings report |
| **Status** | ✅ Complete |

→ [View case study](./Health_Governance_NorthAfrica/README.md) · [View dashboard](./Health_Governance_NorthAfrica/outputs/NorthAfrica_ME_Dashboard.html)

---

### 02 · *Coming soon*

**Deconstructing a FAO Report — NLP & Evidence Mapping**

Planned case study applying text mining, topic modelling, and evidence hierarchy mapping to a FAO thematic report. Goal: extract structured evidence chains, identify methodological gaps, and visualise the logical model underlying the report's conclusions.

| | |
|---|---|
| **Domain** | Evidence synthesis · NLP · Programme evaluation |
| **Methods** | Text mining · Topic modelling · Evidence mapping |
| **Status** | 🔜 In preparation |

---

### More to come…



---

## 🧰 Common Stack

All case studies in this repository share a common Python-based analytical stack:

```
pandas · numpy · scikit-learn · scipy · matplotlib · seaborn
```

Project-specific dependencies are listed in each case study's `README.md`.

---

## 🏗️ Repository Structure

```
real-case-studies/
│
├── README.md                              ← You are here
│
├── Health_Governance_NorthAfrica/         ← Case Study 01
│   ├── data/
│   ├── notebooks/
│   ├── outputs/
│   └── README.md
│
└── [Next_Case_Study]/                     ← Case Study 02 (coming soon)
    ├── data/
    ├── notebooks/
    ├── outputs/
    └── README.md
```

---

## 🌐 Live Dashboards

Interactive dashboards are hosted via GitHub Pages:

| Case Study | Dashboard URL |
|---|---|
| North Africa M&E | [`asmaboufaden.github.io/real-case-studies/Health_Governance_NorthAfrica/outputs/NorthAfrica_ME_Dashboard.html`](https://asmaboufaden.github.io/real-case-studies/Health_Governance_NorthAfrica/outputs/NorthAfrica_ME_Dashboard.html) |

**To activate GitHub Pages:**
Settings → Pages → Source: `Deploy from a branch` → Branch: `main`, folder: `/ (root)` → Save.

---

## 📝 Methodology Principles

All case studies in this repository follow a consistent set of analytical principles:

**1. Transparency first**
AI-assisted tools (Claude AI, Microsoft Copilot) are used for code generation. All outputs, interpretations, and conclusions are verified, reviewed, and validated by the author. This is disclosed in every project.

**2. Methodological rigour**
Every modelling decision is documented with its rationale. Limitations, data gaps, and sources of uncertainty are stated explicitly — not buried in footnotes.

**3. M&E relevance**
Statistical choices are grounded in programme logic and policy relevance, not optimised for metrics alone. A clustering solution that scores marginally lower on silhouette but produces actionable policy segments is preferred over one that is statistically optimal but operationally meaningless.

**4. Reproducibility**
All notebooks are self-contained and executable from raw data to final output. Data sources are public and cited. Random seeds are fixed.

---

## 👩‍💼 Author

**Asma Boufaden**
M&E Specialist | International Development
🌍 North Africa · Francophone Africa
📊 FAOSTAT · World Bank · WHO · UN Data

---

## 📄 License

All case studies are for academic and professional purposes. Public data sources (FAOSTAT, World Bank, WHO, FAO reports) are used under their respective open data licenses. Analytical outputs and visualisations are original work by the author.
