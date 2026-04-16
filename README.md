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
- Key correlations: `trends_PT` lag=2 r=0.938 (Camino Portugués), `trends_FR` lag=0 r=0.85–0.93

### NB02b — Behavioral & Sociological Framework ✅ Complete
- Three pilgrim sociological profiles (Traditional, Secular Seeker, Athletic-Cultural)
- Route behavioral clusters (Challenge / Accessibility / Canonical)
- Structural socio-economic drivers (post-materialism, demographics, low-cost aviation)
- Key behavioral confounds (staged pilgrimage, double counting, COVID decomposition)
- Qualitative sources inventory → `data/qualitative/sources_log.md`

### NB03 — Feature engineering ✅ Complete
- 22 candidate features built across 4 axes from frozen NB01/NB02 schema
- Lags applied per NB02 validated correlations (trends_PT lag=2, trends_KR lag=0/1, etc.)
- Detrended versions (`_dt` suffix) for Axis B signals — common trend confound
- Output: `features_nb03.csv` — 21 years × 42 columns (7 targets + 22 features + 10 `_dt` series)
- Key tautologies flagged and excluded from modelling: `accessibility_routes_share` × `share_p_costa` (r=0.99), `challenge_routes_share` × `share_challenge` (r=1.00)
- **Note**: Trail SJ coverage 2013–2024 reduces effective window to n≈10 for trail-dependent features

### NB04b — Panel Modelling ✅ Complete

Restructures the unit of analysis from annual aggregates (n=21) to a **route × year panel** (n=126, 6 core routes × 21 years). Replaces the share-level approach, which was invalidated by compositional constraints and trend absorption (baseline R²=0.94 from linear trend alone for several targets).

**Panel design:**
- Core panel: Francés, Portugués, Norte, Primitivo, Inglés, Vía de la Plata (6 routes × 21 years = 126 obs)
- Excluded: Invierno, Muxía-Finisterre, Other routes (marginal/extension)
- Treated separately: Camino Portugués Costa (zeros 2004–2015, n=9 usable — §6)
- Estimator: OLS with route fixed effects, HC3 heteroskedasticity-robust standard errors

**Model results:**

| Model | n | R² | adj-R² | Key result |
|-------|---|----|--------|------------|
| M1 — Axis A (climate×route) | 126 | 0.839 | 0.828 | `temp_anomaly_route` p=0.010 ✓ |
| M2 — DiD COVID by route type | 126 | 0.775 | 0.754 | Uniform recovery — no differential |
| M3 — System features + route FE | 114 | 0.865 | 0.850 | `trends_FR_lag0` p=0.000 ✓ |

**Key findings:**

1. **Axis A confirmed: warmer corridors attract more pilgrims (p=0.010).**
A +1°C summer anomaly on a route corridor is associated with +23% pilgrim volume (coef=+0.208). Undetectable at n=21 in share-level modelling. The positive sign applies to Spanish corridors — warmer, drier summers improve walking conditions.

2. **COVID recovery was proportionally uniform across route types.**
No DiD interaction significant (p > 0.25). The rebound is strong (`post_covid` p=0.000) but non-differential — accessible, challenge and canonical routes recovered at the same proportional rate. Profile shifts (P.Costa growth, challenge cluster) are structural trends that pre-date COVID.

3. **`trends_FR_lag0` is the most robust system signal (p=0.000).**
French media interest predicts overall Camino volume across all 6 core routes after controlling for route fixed effects. Confirmed across n=126.

4. **`trends_PT_lag2` is route-specific, not systemic.**
Non-significant in the 6-route panel (p=0.159). It predicts P.Costa and Camino Portugués specifically — a route-level signal, not a system-level driver.

5. **The 2018+ regime is explained by French interest, not an independent structural break.**
`phase_regime 3` loses significance when `trends_FR_lag0` is controlled for. The post-2018 acceleration is media-driven, not an autonomous structural mutation.

### NB05 — Interpretation & dissemination 🔲 Pending
- Quantitative / qualitative triangulation against NB02b behavioral framework
- Regime change narrative: 2004–09 / 2010–17 / 2018+
- Climate mechanism: route-corridor temperature effect (M1) vs Via Podiensis planning-lag (NB02 §4.1)
- Trail culture × challenge cluster behavioral hypothesis (T7, NB04)
- Known limitations: n=126 panel, Trail SJ window, ERA5 proxy corridors, SJPDP data gap
- Research conclusions and stakeholder recommendations

---

## Methodological notes

**On the panel approach:** The route × year panel (NB04b) is the primary modelling framework. Share-level modelling was explored and documented but abandoned — shares are compositionally constrained (sum to 1) and strongly trended, making baseline R² misleadingly high and contextual model interpretation unreliable.

**On sample size:** 126 panel observations (6 routes × 21 years). Trail SJ data (2013–2024) generates 66 NA in trail features — trail-dependent analyses use n≈60. Deep learning or complex architectures are not appropriate at this scale.

**On ERA5 proxies:** Primitivo, Inglés and Vía de la Plata share corridor assignments with Francés or Norte (NB02 §4.1). Temperature coefficients for these routes carry additional measurement uncertainty.

**On the staged pilgrimage confound:** `ratio_sequential` (proxy: `pct_foot / 100`) controls for French→Spanish sequential pilgrimage behaviour. SJPDP departure data unavailable (see NB00 audit).

**On detrending:** Detrended (`_dt`) versions of Axis B features address the common trend confound in NB03. In NB04b, route fixed effects absorb time-invariant route characteristics — detrending of features is less critical but retained for consistency.

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
