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
│   ├── NB00_data_audit.ipynb         # Source mapping & acquisition
│   ├── NB01_collection.ipynb         # Data collection & consolidation
│   ├── NB02_eda.ipynb                # Exploratory data analysis
│   ├── NB03_feature_engineering.ipynb
│   ├── NB04_modeling.ipynb
│   └── NB05_interpretation.ipynb     # Results, SHAP, recommendations
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
- Institutional contact log (AFCC, Xunta de Galice, USC/CETUR, Wikiloc, UTMB,
  American Pilgrims on the Camino, Confraternity of Saint James, SFASJ)
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

### NB02b — Behavioral & Sociological Framework ✅ Complete
- Three pilgrim sociological profiles (Traditional, Secular Seeker, Athletic-Cultural)
- Route behavioral clusters (Challenge / Accessibility / Canonical)
- Structural socio-economic drivers (post-materialism, demographics, low-cost aviation)
- Key behavioral confounds (staged pilgrimage, double counting, COVID decomposition)
- Qualitative sources inventory → `data/qualitative/sources_log.md`

### NB03 — Feature engineering 🔲 Next
- Construction of 22 candidate features across 4 axes
- Detrending and lag engineering
- Feature matrix validation

### NB04 — Modelling 🔲 Pending
- Route share and total pilgrim prediction
- Time-series cross-validation
- Model interpretation and behavioral triangulation

### NB05 — Interpretation & dissemination 🔲 Pending
- Quantitative / qualitative triangulation
- Limitations and known confounds
- Research conclusions

---

## Methodological approach

This project combines quantitative time-series analysis with a
behavioral and sociological interpretive framework (NB02b).
Quantitative findings are systematically triangulated against
pilgrim motivation research, anthropological theory, and
qualitative institutional surveys documented in
`data/qualitative/sources_log.md`.

---

## Key stakeholders (potential users of results)

- **Regional authorities**: Xunta de Galice, Régions Nouvelle-Aquitaine, Occitanie, Auvergne-Rhône-Alpes
- **Pilgrim associations**: AFCC, Amis de Saint-Jacques (SJPDP), American Pilgrims on the Camino
- **Sustainable tourism planners**: UNESCO route management bodies
- **Event organisers**: Trail du Saint-Jacques by UTMB, thematic festivals

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
