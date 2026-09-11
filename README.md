![FreshMart Basket Under Pressure — Key Findings](image.svg/freshmart-findings-infographic.svg)

# FreshMart Basket Decline — Analysis 

**One-line brief:** *"Footfall is up, but our sales aren't growing. It feels like people are buying less per visit."* — Head of Retail Operations

This repo/notebook investigates whether that's true, where it's happening, why, and what to do about it, using the 2025 transactions, stores and stockouts extract (64,330 transactions · 42 stores · 119 stockout events).

> **Note on time frame:** the dataset covers 2025 only, so "before vs after" below compares **H1 2025 (Jan–Jun) vs H2 2025 (Jul–Dec)**, not year-over-year. Directionally it lines up with the business's external "+6% YoY footfall" claim, but this is stated as an assumption, not confirmed against last year's data.

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

More trips, each one smaller, revenue roughly flat-to-down — exactly the arithmetic the Head of Retail Operations suspected.

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
- **Caveats:** single calendar year only (no true YoY comparison); this is a representative *sample* of transactions, not total turnover; all 119 stockout events fall under one category, which may reflect incomplete operational logging rather than a true category-specific issue.
