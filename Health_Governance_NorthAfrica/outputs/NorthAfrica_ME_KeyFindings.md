# North Africa M&E — Key Findings & Policy Insights
## Maternal Mortality Analysis 2000–2023

**Author:** Asma Boufaden | **Data:** FAOSTAT · World Bank WGI · WHO | **Period:** 2000–2023

---

## Executive Summary

This analysis examines maternal mortality trajectories across five North African countries using machine learning, composite development indices, and country clustering. The dominant finding is that **safe water access is the single most powerful structural predictor of maternal mortality** in the region (Pearson r = −0.915), outperforming governance, health expenditure, and food security indicators when considered individually. All five countries have reached the SDG 3.1 target (≤70 deaths per 100,000 live births) as of 2023 — however Libya's trajectory raises serious concerns about sustainability of this achievement through 2030.

---

## Finding 1 — Safe Water Access is the Primary Driver of Maternal Health

**Pearson r = −0.915** between SafeWater_pct and Maternal Mortality — the strongest bivariate association in the dataset.

**What the data shows:**
- Morocco went from 42% safe water access (2000) to 75% (2023), coinciding with a 74.2% reduction in maternal mortality (271 → 70/100k)
- Algeria maintained 70–77% safe water access throughout, consistent with a moderate decline (123 → 62/100k)
- Tunisia's near-stable water access (72–77%) aligns with its more gradual reduction (56 → 36/100k)
- Egypt and Libya lack SafeWater_pct data — their Food Security Index is therefore underestimated

**Policy implication:** Infrastructure investment in safe water systems delivers higher returns on maternal health than health expenditure increases alone. Water access programmes should be a priority intervention for maternal health outcomes in North Africa.

---

## Finding 2 — Morocco: The Most Remarkable Trajectory in the Region

Morocco achieved the **steepest maternal mortality reduction in North Africa: −74.2%** (271 → 70 per 100,000 live births over 23 years), representing the largest absolute improvement in the dataset.

**Three reinforcing mechanisms:**

1. **Safe Water Expansion:** SafeWater_pct rose from 42% (2000) to 75% (2023) — a gain of 33 percentage points, the largest in the region
2. **Health Expenditure Effect:** The **only country in the region where health expenditure is significantly correlated with maternal mortality** (Pearson r = −0.88). This suggests Morocco's health spending translated into genuine maternal care improvements — skilled birth attendance, emergency obstetric care, postnatal services
3. **Life Expectancy Gain:** Morocco achieved the **largest life expectancy improvement in the region: +8.5 years** (66.8 → 75.3 over 23 years), partly explained by higher health expenditure and improved maternal care

**Caution:** Morocco's 2030 projection requires the logarithmic model (not linear). The linear model projects negative values by 2028 — a mathematical floor artefact. The logarithmic model, which captures diminishing returns as mortality approaches low levels, gives an estimated 25.7/100k by 2030.

**Governance context:** Morocco maintained the most stable governance trajectory in the region (avg 67.1, consistently around 65–70 across the period), providing institutional stability for policy implementation.

---

## Finding 3 — Libya: The Only Country at Risk of Missing SDG 3.1 by 2030

**Libya's maternal mortality trajectory has near-zero downward trend** since 2011, and both realistic projection models place it above the SDG 3.1 target of 70/100k by 2030:
- Linear projection 2030: **73.4/100k** (above SDG)
- Logarithmic projection 2030: **73.3/100k** (above SDG)
- Only the polynomial model (R²=0.116, near-zero predictive power) projects Libya below 70

**Root cause: Governance collapse post-2011.** Libya's Governance Index fell from ~20 (pre-2011) to single digits (2015–2016), never recovering. By 2022–2023 it stands at 9.4–11.0 — the lowest in the dataset by a wide margin. The complete institutional breakdown after 2011 dismantled the maternal health delivery infrastructure.

**2020–2021 anomaly:** Libya's maternal mortality spiked to 98/100k in 2021 (the highest single observation in the entire dataset for any country in that year), before falling back to 59 in 2023. This extreme volatility reflects ongoing conflict rather than a genuine epidemiological trend.

**Policy implication:** Libya requires **conflict-sensitive programming** and external support mechanisms. Standard M&E indicators are insufficient for conflict-affected settings. Projections carry extremely high uncertainty (R² ≈ 0.02–0.12 for all models).

---

## Finding 4 — Tunisia: A Governance Success Story with a Fragile Ending

Tunisia demonstrated the region's **strongest governance performance** across the period:
- Governance Index peaked at 89.0 in 2018 — highest in the dataset
- Democratic transition post-2011 translated into better institutional quality
- Health Index average: 77.7 — highest in the region

**However, post-2021 deterioration is concerning:**
- Governance Index fell from 89.0 (2018) → 72.9 (2023), reflecting democratic backsliding
- Maternal mortality spiked to 65/100k in 2021 (COVID-19 and political crisis), before recovering to 36 in 2023
- Projection R² values are low (0.29–0.40) due to this instability — projections carry significant uncertainty

**Life expectancy:** Tunisia's life expectancy gain (+3.9 years, 72.6 → 76.5) is moderate compared to Morocco but its baseline was already the highest in the region.

---

## Finding 5 — Algeria and Egypt: Moderate but Steady Progress

**Algeria:**
- Reduction: −49.6% (123 → 62/100k) in maternal mortality. 
- Governance stagnant (29.8 in 2000 → 52.3 in 2023, never breaking 57)
- Strong Food Security Index (78.2) driven by high SafeWater_pct (70–77%)
- 2020 spike (97/100k) in maternal mortality attributed to COVID-19 — data verified against WHO source
- Linear projection 2030: 58.4/100k (safely below SDG target)

**Egypt:**
- Largest relative reduction after Morocco: −64.6% (48 → 17/100k, 2002–2023)
- Missing SafeWater_pct data throughout — likely underestimated Food Security Index
- Governance declining post-2012 (peaked at 61.8 in 2009, fell to 43.3 by 2021)
- Despite governance decline, maternal mortality continued falling — suggesting strong vertical health programmes (skilled birth attendance, emergency obstetric care)
- Linear projection 2030: 12.1/100k — well below SDG target

---

## Finding 6 — ML Model Insights: Why Tree-Based Models Fail on Small Datasets

The study applies Andrew NG's bias-variance diagnostic framework with a 60/20/20 Train/CV/Test split.

**Core finding:** Tree-based models (Gradient Boosting, Random Forest) **overfit severely** on n=69 observations:
- Gradient Boosting: J_cv/J_train = **222×** (R²_train=1.000, R²_cv=0.936)
- Random Forest: J_cv/J_train = **5×** (R²_train=0.994, R²_cv=0.934)

Despite strong CV scores in absolute terms, the ratios reveal that these models have memorised the training data rather than learning generalisable patterns.

**Winner: Polynomial Regression (degree=2) + Ridge regularisation**
- R²_train = 0.997, R²_cv = 0.985, J_cv/J_train = **3×**
- Captures non-linear feature relationships (SafeWater_pct's diminishing returns)
- Ridge regularisation controls polynomial coefficient explosion
- Achieves the best bias-variance trade-off for this dataset size

---

## Finding 7 — FoodSupply_variability Does Not Predict Maternal Mortality

Feature Set D (low-VIF + FoodSupply_variability) showed no improvement over Feature Set C (low-VIF) across all models. FoodSupply_variability is a **crisis indicator** — it captures episodic food supply shocks, not structural conditions affecting maternal health. It is useful for early warning systems but not for cross-country structural modelling of maternal mortality.

---

## Finding 8 — Health Expenditure is NOT a Universal Predictor

Health expenditure (% of GDP) has a weak correlation with maternal mortality across the full dataset (r = −0.225). The **Morocco exception** (r = −0.88) suggests that the effectiveness of health spending is highly context-dependent:
- In Morocco, spending increases were channelled into maternal care (skilled birth attendance scaled up significantly)
- In other countries, health expenditure may be absorbed by other priorities or lost to inefficiency

**Policy implication:** The amount spent on health matters less than *where* it is directed. Targeted maternal care investments yield greater returns than general health budget increases.

---

## Composite Indices Summary

| Country | Avg Governance | Avg Health | Avg Food Security | Cluster |
|---|---|---|---|---|
| **Tunisia** | 79.4 | 77.7 | 82.9 | High Performers |
| **Morocco** | 67.1 | 49.3 | 66.8 | Mid-level Developers |
| **Algeria** | 52.3 | 70.2 | 78.2 | High Performers |
| **Egypt** | 51.9 | 55.4 | 66.2* | Mid-level Developers |
| **Libya** | 17.4 | 56.1 | 68.9* | Fragile / Post-conflict |

*Underestimated — SafeWater_pct missing

**Governance:** Tunisia leads (avg 79.4), Morocco rising steadily (67.1). Libya collapses post-2011 to near-zero. Algeria and Egypt stagnate in the 50s.

**Health:** Tunisia strongest (77.7) — high life expectancy + health expenditure + low maternal mortality. Morocco surprisingly low (49.3) due to high maternal mortality baseline in 2000, despite dramatic improvement.

**Food Security:** Tunisia (82.9) and Algeria (78.2) lead. Libya, Morocco, Egypt cluster around 66–69. Note: Egypt and Libya Food Security Indices are underestimated due to missing SafeWater data.

---

## SDG 3.1 Status and 2030 Outlook

| Country | 2023 MM | SDG Status | 2030 Projection (log) | 2030 Outlook |
|---|---|---|---|---|
| Algeria | 62 | ✅ Achieved | 64.0 | ✅ Maintained |
| Egypt | 17 | ✅ Achieved | 16.3 | ✅ Maintained |
| **Libya** | **59** | **✅ Achieved** | **73.3** | **⚠️ At risk** |
| Morocco | 70 | ✅ Achieved | 25.7 | ✅ Maintained |
| Tunisia | 36 | ✅ Achieved | 37.0 | ✅ Maintained |

---

## Policy Recommendations

1. **Scale up safe water access** — the single highest-leverage intervention for maternal health in the region. Prioritise Egypt and rural Morocco (final percentile gains are hardest)

2. **Morocco model for replication** — the combination of water infrastructure + targeted health spending + governance stability created a virtuous cycle. Egypt and Algeria should study this model

3. **Libya requires conflict-sensitive M&E** — standard projections are meaningless in this context. Establish baseline assessments tied to political stabilisation milestones

4. **Protect Tunisia's gains** — the post-2021 governance deterioration threatens health system performance. M&E monitoring should flag this as an early warning

5. **Direct health expenditure toward maternal care** — not general health budgets. Morocco shows that targeted spending (skilled birth attendance, emergency obstetric care) translates into measurable outcomes

6. **Address data gaps for Egypt and Libya** — SafeWater_pct is missing for both countries, underestimating their Food Security Index and limiting model performance for these countries

---

*Document generated as part of the North Africa M&E Predictive Analysis Project | Data: 2000–2023 | FAOSTAT · World Bank WGI · WHO*
