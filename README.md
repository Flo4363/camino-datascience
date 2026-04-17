# Camino de Santiago — Data Science Project

> **What contextual, non-demographic factors explain and predict the structural mutations of the Camino de Santiago between 2003 and 2025?**

---

## Overview

This project investigates the **exogenous drivers** of pilgrim flow changes on the Camino de Santiago (and French pilgrimage routes) using a multi-source, longitudinal dataset spanning 2003–2025.

Rather than replicating existing demographic profiling studies, this work focuses on four underexplored axes:

| Axis | Focus |
|------|-------|
| **A — Climate sensitivity** | Impact of weather anomalies on route choice and monthly flow |
| **B — Media effect** | Correlation between films, books, YouTube/Vimeo content, trail events and pilgrim waves by country |
| **C — Geographic diversification** | Modelling the shift from Camino Francés to secondary routes (Spain + France) |
| **D — Resilience & recovery** | Post-crisis rebound modelling by segment (COVID, Holy Years, economic shocks) |

---

## Research scope

- **Geographic coverage**: Spain (all official routes) + France (Via Podiensis, Turonensis, Lemovicensis, Tolosana + regional branches)
- **Time range**: 2003–2025 (annual and monthly granularity)
- **Primary data source**: Oficina del Peregrino, Santiago de Compostela
- **Complementary sources**: AFCC (France), SJPDP office (Saint-Jean-Pied-de-Port), ERA5 climate data, Google Trends, UTMB Trail du Saint-Jacques results, Wikiloc, social media signals

---

## Project structure

```
camino-datascience/
├── data/
│   ├── raw/          # Original data, never modified
│   ├── processed/    # Cleaned and transformed datasets
│   └── external/     # Third-party data (climate, trends, trail...)
├── notebooks/
│   ├── NB00_data_audit.ipynb
│   ├── NB01_collection.ipynb
│   ├── NB02_eda.ipynb
│   ├── NB02b_behavioral_framework.ipynb
│   ├── NB03_feature_engineering.ipynb
│   ├── NB04b_panel_modelling.ipynb
│   └── NB05_interpretation.ipynb
├── src/              # Reusable Python modules
├── reports/          # Exported figures and outputs
├── README.md         # This file (English)
└── README.fr.md      # French summary (for French-speaking partners)
```

> **Note on data**: Raw and processed data files are excluded from version control (see `.gitignore`). To reproduce the dataset, follow the instructions in `NB00_data_audit.ipynb`.

---

## Notebooks roadmap

### NB00 — Data audit & acquisition ✅ Complete
- Inventory of all data sources (Tier 1 / Tier 2 / Tier 3)
- Institutional contact log (AFCC, Xunta de Galice, USC/CETUR, Wikiloc, UTMB, American Pilgrims on the Camino, Confraternity of Saint James, SFASJ)
- Contact email templates (FR + EN)
- Repository setup and conventions

### NB01 — Data collection ✅ Complete
- **Section 1** — Google Trends: digital visibility index, 9 countries, 2004–2024
- **Section 2** — Oficina del Peregrino: annual totals, routes, nationalities (PDF 2004–2018 + manual 2019–2024)
- **Section 3** — Trail du Saint-Jacques by UTMB: finisher data, 11 editions (2013–2024)
- **Section 4** — ERA5 Climate: monthly temperature + precipitation, 5 route corridors, 2004–2024

### NB02 — Exploratory Data Analysis ✅ Complete
- Cross-source dashboard across all collected signals (2004–2024)
- Structural break detection — PELT algorithm (Holy Years, COVID, UTMB integration)
- Cross-source correlation analysis — Pearson lag=0 and lagged (lag 1–2)
- Axis A/B/C/D hypothesis validation — all-routes approach
- Key correlations: `trends_PT` lag=2 r=0.938 (Camino Portugués), `trends_FR` lag=0 r=0.85–0.93

### NB02b — Behavioral & Sociological Framework ✅ Complete
- Three pilgrim sociological profiles (Traditional, Secular Seeker, Athletic-Cultural)
- Route behavioral clusters (Challenge / Accessibility / Canonical)
- Structural socio-economic drivers (post-materialism, demographics, low-cost aviation)
- Key behavioral confounds (staged pilgrimage, double counting, COVID decomposition)

### NB03 — Feature engineering ✅ Complete
- 22 candidate features built across 4 axes from frozen NB01/NB02 schema
- Lags applied per NB02 validated correlations (trends_PT lag=2, trends_KR lag=0/1, etc.)
- Detrended versions (`_dt` suffix) for Axis B signals — common trend confound
- Output: `features_nb03.csv` — 21 years × 42 columns (7 targets + 22 features + 10 `_dt` series)

### NB04b — Panel Modelling ✅ Complete

Route × year panel (n=126, 6 core routes × 21 years). Replaces share-level modelling (compositional bias documented).

| Model | n | adj-R² | Key result |
|-------|---|--------|------------|
| M1 — Axis A (climate×route) | 126 | 0.828 | `temp_anomaly_route` p=0.010 ✓ |
| M2 — DiD COVID by route type | 126 | 0.754 | Uniform recovery — no differential |
| M3 — System features + route FE | 114 | 0.850 | `trends_FR_lag0` p=0.000 ✓ |

### NB05 — Interpretation & Dissemination ✅ Complete

Final analytical notebook: findings narrative, behavioural triangulation, phase interpretation, limitations, stakeholder recommendations.

**Research question — answer:**
The dominant factor explaining structural mutations of the Camino de Santiago is **French media interest** — a continuous, measurable demand signal driving system-wide volume and explaining the post-2018 acceleration without requiring an exogenous structural break. Climate operates at the route-corridor level (+23% per +1°C anomaly). Trail culture partially substitutes for challenge-route demand. Structural crises (COVID, Holy Years) amplify the existing trajectory rather than redirecting it.

**Five key findings:**

1. **Axis A confirmed:** +1°C summer anomaly on a corridor → +23% pilgrim volume (p=0.010)
2. **COVID recovery uniform:** no differential by route type — pent-up demand release affects all routes proportionally
3. **`trends_FR_lag0` dominant system signal** (p=0.000) — French media interest predicts system-wide volume
4. **`trends_PT` is route-specific:** predicts Portuguese route cluster, not the full system (p=0.159 in panel)
5. **2018+ phase is media-driven:** loses significance when `trends_FR` is controlled for — not an autonomous structural break

**Outputs:** `NB05_recommendations.csv` · 4 figures (R² summary, triangulation heatmap, phase narrative, limitations)

---

## Methodological notes

**On the panel approach:** Route × year panel (NB04b) is the primary modelling framework. Share-level modelling was explored and documented but abandoned — shares are compositionally constrained and strongly trended.

**On sample size:** 126 panel observations (6 routes × 21 years). Trail SJ data (2013–2024) generates 66 NA — trail-dependent analyses use n≈60.

**On ERA5 proxies:** Primitivo, Inglés and Vía de la Plata share corridor assignments with Francés or Norte (NB02 §4.1). Temperature coefficients for these routes carry additional measurement uncertainty.

**On the staged pilgrimage confound:** `ratio_sequential` (proxy: `pct_foot / 100`) controls for French→Spanish sequential behaviour. SJPDP departure data unavailable (NB00).

---

## Key stakeholders

- **Regional authorities**: Xunta de Galice, Régions Nouvelle-Aquitaine, Occitanie, Auvergne-Rhône-Alpes
- **Pilgrim associations**: AFCC, Amis de Saint-Jacques (SJPDP), American Pilgrims on the Camino
- **Sustainable tourism planners**: UNESCO route management bodies
- **Event organisers**: Trail du Saint-Jacques by UTMB

---

## Language convention

- **Code, comments, notebooks**: English
- **README.fr.md**: French summary for francophone partners
- **Data source documents**: kept in original language (ES/FR/EN) with translation notes where needed

---

## Author

Florian — Data Analyst & Data Scientist (MINES Paris PSL)
Personal passion for the Camino de Santiago since the 1990s.

---

## License

MIT License — see `LICENSE` for details.
