
![FreshMart Basket Under Pressure — Key Findings](image.svg/freshmart-findings-infographic.svg)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=postgresql&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=flat&logo=databricks&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)

# FreshMart Basket Decline — Analysis

**One-line brief:** *"Footfall is up, but our sales aren't growing. It feels like people are buying less per visit."* — Head of Retail Operations

This repo/notebook investigates whether that's true, where it's happening, why, and what to do about it, using the 2025 transactions, stores and stockouts extract (64,330 transactions · 42 stores · 119 stockout events).

> **Note on time frame:** the dataset covers 2025 only, so "before vs after" below compares **H1 2025 (Jan–Jun) vs H2 2025 (Jul–Dec)**, not year-over-year. Directionally it lines up with the business's external "+6% YoY footfall" claim, but this is stated as an assumption, not confirmed against last year's data.

---

## Project Links

| Deliverable | Link |
|---|---|
| 📊 Power BI dashboard | `[Paste your published Power BI report URL here]` |
| 🖥️ Presentation deck | `[Paste your slide deck / PDF link here]` |
| 🧮 SQL scripts | `[Paste your SQL repo folder or gist link here]` |
| 🧱 Databricks dashboard | [View dashboard](https://dbc-a371be58-89fc.cloud.databricks.com/dashboardsv3/01f1adc24c1919d8b1132c6d364508ce/published?o=7474644403956981) |
| 📄 Full analytical workflow | [WORKFLOW.md](WORKFLOW.md) — step-by-step, from business case to recommendation |

---

## 1. Who this is for (stakeholders)

| Stakeholder | What they care about |
|---|---|
| Head of Retail Operations | Is the basket-shrinkage story real, how big, and what to do first |
| Merchandising Lead | Whether product mix / availability is driving smaller baskets |
| Pricing Manager | Whether pricing or competitor pricing pressure is the cause |
| Store Managers | Store- and operations-level issues (stockouts, local competition) they can act on directly |

---

## 2. Headline finding

**The paradox is real and confirmed in the data — and it's sharply worse near new discount competitors.**

| Metric | H1 2025 | H2 2025 | Change |
|---|---|---|---|
| Transaction volume | 31,109 | 33,221 | **+6.8%** |
| Avg. basket value (ZAR) | R290.01 | R261.10 | **‑10.0%** |
| Avg. items per basket | 14.2 | 12.7 | **‑10.6%** |
| Total revenue (ZAR) | R9.02m | R8.67m | **‑3.9%** |

Headline: customers are still visiting, but they are purchasing fewer items per visit. More trips, each one smaller, revenue roughly flat-to-down — exactly the arithmetic the Head of Retail Operations suspected.

### Root-cause signals

- 41 of 42 stores had a lower H2 average basket than H1.
- Competitor-exposed stores fell from about R285.58 before competitor opening to R225.09 after (-21.18%).
- Stockout store-days averaged about R231.95 versus R276.31 on normal store-days (-16.05%).
- Loyalty members had larger baskets and a smaller decline than non-loyalty shoppers.
- Value per item increased slightly, so pricing/value deterioration is not the primary driver.
- Competition and stockouts are strong associations, not proof of causality. A treatment/control pilot is recommended.

| Hypothesis | Result |
|---|---|
| Fewer items | 🟢 Supported |
| Competition | 🟢 Strong signal |
| Stockouts | 🟢 Strong signal |
| Pricing | 🔴 Weak |
| Loyalty | 🟡 Contributing |

The evidence points towards customer behaviour amplified by competitive and availability pressures.

---

## 3. Key insights

1. **Chain-wide, not isolated.** The basket decline shows up across all three provinces and all three store formats (roughly ‑9% to ‑11% everywhere) — this is not one region or one format's problem.

2. **Competitor proximity is the single strongest driver found.** The 12 stores where a discount competitor opened nearby fell from an average basket of **R285.58 before** the opening to **R225.09 after** (‑21.2%). Stores with no nearby competitor fell only ‑7.2% (R289.65 → R268.80) over the same window. Competitive intrusion is roughly **3x** the baseline erosion rate.

3. **Shoppers are trading big weekly shops for small top-up trips.** The share of baskets under R150 rose from 34.4% (H1) to 41.7% (H2), while baskets of R300–800 fell from 50.0% to 42.8%. This is a genuine mix-shift in shopping behaviour, not just a shrinking average.

4. **Loyalty cushions the decline but doesn't stop it.** Loyalty members still spend more per basket (R283.58 vs R243.48 in H2) and declined more slowly (~‑7%) than non-members (~‑12%). 55% of all transactions have no loyalty ID attached — a large addressable non-member base.

5. **Stockouts are a real but small-scale drag.** 119 recorded events (all logged under one category, "Household Cleaning" — itself a data-quality flag suggesting incomplete logging) affected only ~0.9% of transactions, but on those store-days average basket value was 15.7% lower (R232 vs R275). Too small in volume to explain the chain-wide trend, but a fixable operational leak.

6. **Data quality note:** the 55.4% blank `customer_id` field is expected (non-loyalty shoppers, per the data dictionary), not a missing-data problem — worth stating explicitly so it isn't mistaken for one.

---

## 4. Recommendations & expected business impact

| # | Recommendation | Why (evidence) | Expected impact | Owner |
|---|---|---|---|---|
| 1 | Competitive response package (price-match / targeted promos / loyalty offers) at the 12 exposed stores, prioritised by exposure date | Competitor-exposed stores decline 3x faster than baseline | Highest-leverage single fix — directly targets the steepest driver | Pricing Manager + affected Store Managers |
| 2 | Cross-merchandising / multi-buy promos to convert small top-up trips into larger baskets, especially in Express format | Mix has shifted toward sub-R150 trips | Recovers basket value without needing more footfall | Merchandising Lead |
| 3 | Loyalty sign-up push targeting the 55% non-loyalty transaction base | Non-members spend less and are declining faster | Larger, more resilient basket base over time | Head of Retail Ops / Marketing |
| 4 | Fix stockout logging (expand beyond one category) and prioritise availability on high-velocity lines | Stockout days show a real, if small, basket hit | Removes an avoidable revenue leak | Store Operations |
| 5 | Stand up a monthly monitoring dashboard (basket value, items/trip, basket-size mix, loyalty penetration, stockout incidence — split by competitor exposure) | Enables an ongoing, fair before/after read | Turns this from a one-off study into an early-warning system | BA / Analytics |

---

## 5. Success metrics & how to test

- **Primary:** average basket value (ZAR) and average items per trip, tracked monthly, split by store and by competitor-exposure status.
- **Secondary:** basket-size band mix (% small vs large trips), loyalty penetration rate, stockout incidence and category coverage.
- **Test design:** treat the 12 competitor-exposed stores as the treatment group; use a matched set of similar stores (same format/province, no nearby competitor) as control. Compare basket trends before vs after each intervention, the same before/after design used to isolate the competitor effect above.

---

## 6. Data & method notes (for reproducibility)

- **Source:** `FreshMart_Dataset.xlsx` — `Transactions` (64,330 rows), `Stores` (42 rows), `Stockouts` (119 rows), joined on `store_id`.
- **Derived fields used:** `half` (H1/Jan–Jun vs H2/Jul–Dec), `basket_value_band`, stockout-day flag (store + date match against Stockouts table), competitor before/after flag (transaction date vs each store's `competitor_open_date`).
- **Full step-by-step workflow:** see [WORKFLOW.md](WORKFLOW.md) for how each finding above was derived — business questions, data-quality checks, SQL logic, and the reasoning behind each hypothesis test.

**Tools**

- Excel — exploration and dashboard prototyping
- Power BI — interactive executive reporting
- SQL / Databricks — reproducible analysis
- Python / Pandas — cleaning and analysis
- GitHub — portfolio documentation

---

## Data Quality

The dataset contains 64,330 transaction records, 42 stores and 119 stockout events. Blank customer IDs represent non-loyalty transactions in the case dataset. Findings are based on the supplied sample and should be validated against official operational/financial systems before being treated as actual company turnover or causal impact.

---

## Portfolio Structure

```
FreshMart-Basket-Analytics/
├── data/
├── excel/
├── powerbi/
├── sql/
├── python/
├── images/
├── WORKFLOW.md
└── README.md
```
# FreshMart Supermarkets — Why Is Our Basket Getting Smaller?

A SQL-based investigation into declining transaction value across FreshMart's 42-store chain

## The brief

> "Footfall is up, but our sales aren't growing. It feels like people are buying less per visit. Find out what is going on, and tell us what to do about it."
> — Head of Retail Operations, FreshMart Supermarkets

## Dataset

Three linked tables for the 2025 calendar year, joined on `store_id`:

| Table | Grain | Rows |
|---|---|---|
| `Transactions` | One row per basket | 64,330 |
| `Stores` | One row per store | 42 |
| `Stockouts` | One row per stockout event | 119 |

## Problem framing

"Basket size" was defined two ways — **average basket value (ZAR)** and **average items per basket** — since they can move independently and tell different stories. Both were tracked over calendar months and compared first half (Jan–Jun) vs second half (Jul–Dec) of 2025.

**The decline is real and chain-wide.** Average basket value fell from R290.01 (H1) to R261.10 (H2) — a **10.0% decline** — while transaction volume rose (31,109 → 33,221), confirming the footfall-up/revenue-flat arithmetic in the brief. Average items per basket fell in step, from 14.22 to 12.72, while average price per item stayed essentially flat (~R20.30–R20.60 all year). This rules out a simple pricing/discounting explanation: **customers are putting fewer items in the basket, not paying less per item.**

## Hypotheses tested

| # | Hypothesis | Evidence checked | Verdict |
|---|---|---|---|
| 1 | Pricing — items falling because prices dropped | Avg price per item by month | **Rejected.** Price per item stable to slightly rising all year. |
| 2 | Nearby competitor entry pulling spend away | Basket value before vs after each store's competitor-open date (±60 day window) | **Supported — strongly.** -18.2% around competitor openings, nearly double the chain-wide rate. |
| 3 | Product availability (stockouts) reducing basket size | Basket value on stockout weekends vs normal weekends, at the 6 affected stores | **Supported, but narrow.** -18.5% impact, but limited to 6 of 42 stores and one category (Household Cleaning). |
| 4 | Changing shopping behaviour — more frequent, smaller trips replacing big weekly shops | Share of Small/Medium/Large basket bands, H1 vs H2 | **Supported.** Small-basket share rose from 34.4% to 41.7%; Large-basket share fell from 31.7% to 26.7% — a genuine mix shift, not just smaller averages everywhere. |
| 5 | Loyalty status masking or driving the decline | Basket value by loyalty status, H1 vs H2 | **Partially supported.** Both groups declined, but non-members fell faster (-12.3%) than loyalty members (-7.0%). |
| 6 | Day-of-week / weekday vs weekend effect | Basket value by day type, H1 vs H2 | **Rejected.** Weekday (-10.3%) and weekend (-9.4%) declines are essentially the same. Red herring. |
| 7 | Store format or province effect | Basket value by format and province, H1 vs H2 | **Weak effect.** Large stores (-11.2%) and Gauteng (-11.6%) dipped slightly more, consistent with Gauteng carrying 7 of the 12 competitor openings — not an independent driver on its own. |

## Root cause

The decline is not one single cause but **two layers**:

1. **A broad, chain-wide behavioural shift** (~10% decline): customers are making more, smaller trips instead of fewer, larger ones. Items per basket are falling; price per item is not. This affects all 42 stores and both loyalty segments, non-members more than members.
2. **Concentrated, high-impact operational events** riding on top of that trend: stores that gained a nearby discount competitor lost basket value at roughly **twice** the chain-wide rate (-18.2%) in the weeks around the competitor's opening, and the 6 stores suffering repeated weekend stockouts lost a similar amount (-18.5%) specifically on the affected weekend days.

Gauteng and Large-format stores show the sharpest overall declines mainly because that's where competitor entries were concentrated (7 of 12 in Gauteng) — not because province or format is an independent cause.

## Data quality notes

- No missing values found in the key numeric fields (`basket_value_zar`, `num_items`) across 64,330 transaction rows.
- `customer_id` is blank for non-loyalty shoppers by design (35,674 of 64,330 transactions) — treated as a category, not a data-quality gap.
- No negative or zero basket values / item counts found.
- All 12 competitor openings and all 119 stockout events fall in the second half of 2025 — this asymmetry is real in the source data, not a coverage gap (data covers the full year for all three tables).
- All 119 stockout events share one category (Household Cleaning), one root cause noted ("Replenishment gap – weekend delivery missed"), and land on Saturday/Sunday only — a narrow, well-defined operational failure rather than a broad inventory problem.

## Recommendations

| Priority | Action | Expected impact | Owner | Risk |
|---|---|---|---|---|
| 1 | Fix the weekend replenishment gap for Household Cleaning at the 6 affected stores (adjust delivery scheduling to cover Sat/Sun) | Recovers ~18% basket value on affected weekend days at those 6 stores | Store Ops / Supply Chain | Low — isolated, well-understood cause |
| 2 | Launch a targeted retention offer (loyalty sign-up push + basket-building promotions) at the 12 stores with a nearby competitor, timed around the opening | Cushions the ~18% competitor-driven drop; loyalty members already decline slower (-7% vs -12%) | Merchandising / Loyalty team | Medium — needs budget, competitor may respond |
| 3 | Investigate the small-basket mix shift chain-wide: are these legitimate top-up trips (e.g. convenience missions) that need a different assortment/pricing strategy, or lost big-basket occasions to be won back | Addresses the largest-volume driver (all 42 stores) | Merchandising / Pricing | Medium — needs further qualitative research (e.g. customer survey) to confirm the "why" |
| 4 | Extend competitor-tracking and stockout monitoring chain-wide, not just reactively | Enables earlier response to future competitor entries or supply gaps | Retail Operations | Low |

## Success metrics & monitoring

- **Primary metric:** average basket value (ZAR) and average items per basket, tracked monthly, chain-wide and by store.
- **Fair test for interventions:** treatment vs control store comparison (e.g. stores getting the retention offer vs matched stores that don't), before/after the intervention date — the same before/after logic used to detect the competitor effect in this analysis.
- **Monitoring dashboard should track:** monthly avg basket value & items/basket, small/medium/large basket-band mix (%), loyalty vs non-loyalty basket value, and a live flag for any store with a newly opened nearby competitor or an active stockout, so these events can be responded to as they happen rather than discovered a quarter later.

## Limitations

- This is a representative sample of transactions, not the full population — used to detect patterns and relative differences, not to report FreshMart's exact total turnover.
- The competitor and stockout effects are observed from before/after comparisons rather than a designed experiment; other factors coinciding with those dates cannot be fully ruled out.
- The mix-shift hypothesis (more small trips) is well evidenced in the data but the underlying *reason* customers are shifting trip type is not directly observable here and would benefit from qualitative follow-up (e.g. customer survey).

## Repo contents

- `FreshMart_Analysis.sql` — full SQL analysis script, organized by task
- `README.md` — this summary

**Caveats:** single calendar year only (no true YoY comparison); this is a representative *sample* of transactions, not total turnover; all 119 stockout events fall under one category, which may reflect incomplete operational logging rather than a true category-specific issue.
