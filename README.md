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

### NB00 — Data audit & acquisition *(current phase)*
- Inventory of all data sources
- Classification: freely available / requires contact / likely inaccessible
- Contact log for institutional data requests (AFCC, USC/CETUR, Xunta de Galice, Départements, UTMB)
- Repository setup and conventions

### NB01 — Data collection
- Scraping/downloading Oficina del Peregrino PDFs (2003–2025)
- ERA5 climate data extraction by route corridor
- Google Trends series by country and language
- SJPDP statistics (Saint-Jean-Pied-de-Port)
- Trail du Saint-Jacques finisher data (LiveTrail, 2012–2024)
- Media event database (manual construction)

### NB02 — Exploratory data analysis
- Long time-series visualisation
- Structural break detection (COVID, Holy Years)
- Initial correlations between contextual features and flows
- Axis validation or invalidation

### NB03 — Feature engineering
- Composite climate index per route and season
- Lag variables for media effect (6, 12, 18-month windows)
- Saturation indicator (Francés/total ratio)
- Binary/graduated crisis variables
- Digital visibility index (YouTube, Google Trends, trail signal)

### NB04 — Modelling
- Global interpretable model (LightGBM + SHAP) for feature ranking
- Axis-specific models
- Time-series component (Prophet / SARIMA) for predictive dimension
- Evaluation: MAE, RMSE, feature importance

### NB05 — Interpretation & dissemination
- SHAP value narrative
- Geospatial visualisations (flow maps by route)
- Limitations and bias documentation (Compostela-only bias, pre-2003 gaps)
- Operational recommendations for stakeholders

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
