# American Pilgrims on the Camino (APOC) — Synthesis note

**Source**: https://americanpilgrims.org/statistics/
**Retrieved**: April 2026 (2025 edition, published January 2026)
**Relevance**: Axis B (media/demand), NB02b (pilgrim profiles), NB03 (feature candidates)

---

## 1. Global statistics 2025

| Indicator | Value | Source |
|-----------|-------|--------|
| Total Compostelas 2025 | 530,919 | Oficina del Peregrino |
| YoY growth 2025 | +6% | Oficina |
| Growth vs 10 years ago | +90% | Oficina |
| US pilgrims share | 8% (2nd nationality) | Oficina |
| Spanish pilgrims share | 42% (1st nationality) | Oficina |
| % walking (foot) | 93% | Oficina |
| Pilgrims starting at Sarria | 31% of total | Oficina |
| Starting at 100km threshold points | 51% of total | Oficina |

**NB03 action**: update master_annual.csv with 2025 data point (530,919).
Compare with 2024 (499,186) — confirms continued growth in new regime.

---

## 2. US pilgrim profile — key sociological data

| Indicator | US pilgrims | Global average |
|-----------|-------------|----------------|
| % Women | 61% | 54% |
| % Age 46+ | 71% | 50% |
| Starting from SJPDP | 2x more likely than average | — |
| APOC credential requests 2025 | 13,997 | — |
| APOC credential requests growth 2025 | +14% vs 2024 | — |
| APOC credential growth over 10 years | +96% | — |

### Interpretation for NB02b — Profile B confirmation

The US pilgrim profile strongly confirms Profile B (secular seeker):
- 61% women vs 54% globally — feminisation more advanced in US segment
- 71% age 46+ vs 50% globally — retirement/life transition triggers dominant
- 2x more likely to start SJPDP — "integral" pilgrimage behavior (The Way template)
- These pilgrims are NOT starting in Sarria — they commit to the full Spanish section

This directly supports the behavioral hypothesis that long-distance country pilgrims
(US, KR, BR) disproportionately adopt the "integral" Profile B pattern vs European
sequential pilgrims who may start at various points along French routes.

---

## 3. Route dynamics 2025 — Axis C confirmation

| Route | YoY growth 2025 |
|-------|-----------------|
| Camino Portugués Costa | +20% |
| Camino Primitivo | +14% |
| Camino Inglés | +8% |
| Camino Francés | +2% |

**Key finding**: For the third consecutive year, Portuguese routes combined grow
faster than the Francés. The P.Costa +20% in 2025 confirms the structural trend
identified in NB02 — the plateau at diversity ~0.70 is NOT a stabilisation of P.Costa
share but a continuing internal recomposition within the plateau.

**NB03 implication**: the growth_momentum feature (3-year rolling index) will show
P.Costa still accelerating in 2025 even as the diversity_core plateaus.

---

## 4. Compostela counting bias — confirmed

APOC confirms the ~40% undercount finding from the Xunta IoT study:
- St. Jean office counted 54,115 pilgrims in 2025
- Santiago only counted 30,345 from the same cohort
- 23,770 extra pilgrims stopped before Santiago

This is independent corroboration of the Xunta sensor study finding.
The bias is consistent across years — trend analysis remains valid.

---

## 5. New Compostela rules (2025) — potential future signal

The Pilgrim's Reception Office changed Compostela requirements in 2024/2025,
allowing pilgrims to start further back on routes and still qualify.
APOC notes this "could start seeing a shift" in starting point distribution.

**Watch for NB03**: if 2025 shows shift in pct_start_sarria, this is a supply-side
institutional change that would need a feature flag (compostela_rules_change = 1 for 2025+).
This is analogous to sanitary_regime for COVID.

---

## 6. APOC credential requests — potential NB03 feature

APOC collects annual credential request data which represents ~30% of US pilgrims
who received a Compostela. This is a **quantified absolute demand proxy** for US
pilgrim interest — superior to Google Trends US (relative index only).

**Feature candidate**: `credential_requests_US` (annual count)
**Availability**: conditional on APOC sharing historical series (contact initiated April 15th 2026 — see sources_log.md)
**Expected series**: 2010–2025 (APOC founded 2003, credential program active since ~2010)

If obtained, this feature would allow:
- Direct test of The Way film effect (2010-2012 spike)
- Quantified US demand leading indicator for total pilgrim growth
- Decomposition of trends_US Trends signal into quantified absolute demand

---

## 7. Data not available on this page

- Annual breakdown of credential requests by year (historical series) — to request
- Route breakdown for US pilgrims specifically
- Motivation data for US pilgrims
- Repeat pilgrim rate for US segment
