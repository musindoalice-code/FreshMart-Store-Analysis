
![FreshMart Basket Under Pressure — Key Findings](image.svg/freshmart-findings-infographic.svg)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=postgresql&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=flat&logo=databricks&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)


# FreshMart Supermarkets — Why Is Our Basket Getting Smaller?

FreshMart is a 42-store South African grocery chain. Leadership noticed something odd: more customers were coming through the door, but sales weren't growing. Their theory was that people were buying less each time they shopped — but nobody could say how much, where, or why. This project investigates that question using a year of transaction, store, and stockout data, and turns the answer into a set of concrete recommendations.

> "Footfall is up, but our sales aren't growing. It feels like people are buying less per visit. Find out what is going on, and tell us what to do about it."
> — Head of Retail Operations, FreshMart Supermarkets

**A note on the time frame:** the dataset only covers 2025, so every "before vs after" comparison below is **first half of 2025 (Jan–Jun) vs second half (Jul–Dec)**, not a year-over-year comparison. It lines up directionally with the business's separate claim of +6% year-on-year footfall, but that claim itself isn't something this dataset can confirm.

---

## Project links

| Deliverable | Link |
|---|---|
| 📊 Power BI dashboard | `[paste your published Power BI report URL here]` |
| 🖥️ Presentation deck | `[paste your slide deck / PDF link here]` |
| 🧮 SQL scripts | [`FreshMart_Analysis.sql`](FreshMart_Analysis.sql) |
| 🧱 Databricks dashboard | [View dashboard](https://dbc-a371be58-89fc.cloud.databricks.com/dashboardsv3/01f1adc24c1919d8b1132c6d364508ce/published?o=7474644403956981) |
| 📄 Full analytical workflow | [WORKFLOW.md](WORKFLOW.md) — step-by-step, from business question to recommendation |

---

## Who this is for

| Stakeholder | What they care about |
|---|---|
| Head of Retail Operations | Whether the "shrinking basket" story is real, how big it is, and what to fix first |
| Merchandising Lead | Whether product mix or availability is behind smaller baskets |
| Pricing Manager | Whether pricing, or competitor pricing pressure, is the cause |
| Store Managers | Store-level, operational issues (stockouts, local competition) they can act on directly |

---

## Headline finding

The concern turned out to be real — and worse in specific, identifiable spots than on average.

| Metric | H1 2025 | H2 2025 | Change |
|---|---|---|---|
| Transactions | 31,109 | 33,221 | +6.8% |
| Avg. basket value | R290.01 | R261.10 | **-10.0%** |
| Avg. items per basket | 14.2 | 12.7 | -10.6% |
| Total revenue | R9.02m | R8.67m | -3.9% |

In plain terms: customers are still coming in — more of them, even — but each visit brings home less. 41 of the 42 stores saw their average basket value fall from the first half of the year to the second. Average price per item barely moved (roughly R20.30–R20.60 all year), so this isn't about people buying the same amount for less money — they're genuinely putting fewer items in the basket.

---

## What we tested, and what we found

| Hypothesis | What we checked | Result |
|---|---|---|
| Prices dropped | Average price per item, by month | **Ruled out.** Price per item was flat to slightly rising all year. |
| A nearby competitor opened | Basket value in the ~2 months before vs after each store's own competitor-opening date | **Strong driver.** Baskets fell 18.2% around competitor openings — nearly double the chain-wide rate. |
| Stockouts hurt basket size | Basket value on stockout weekends vs normal weekends, at the affected stores | **Real, but narrow.** An 18.5% drop, but confined to 6 of 42 stores and a single category (Household Cleaning). |
| Shopping habits are changing | Share of small/medium/large baskets, H1 vs H2 | **Confirmed.** Small baskets (under R150) grew from 34% to 42% of all trips; large baskets (R400+) shrank from 32% to 27%. People are swapping big weekly shops for small top-up trips. |
| Loyalty membership matters | Basket value by loyalty status, H1 vs H2 | **Partly.** Both groups declined, but non-members fell faster (-12.3%) than loyalty members (-7.0%). |
| It's a weekday/weekend thing | Basket value by day type, H1 vs H2 | **Ruled out.** Weekday and weekend declines were almost identical — a red herring. |
| Store format or province matters on its own | Basket value by format and province, H1 vs H2 | **Mostly ruled out.** Large-format stores and Gauteng dipped a bit more than average, but only because that's where most competitor openings happened to land — not an independent cause. |

---

## So what's actually driving this?

Two things, layered on top of each other:

1. **A broad shift in shopping behaviour, affecting the whole chain.** People are making more, smaller trips instead of fewer, bigger ones. This accounts for the baseline ~10% decline seen almost everywhere, and it hits non-loyalty shoppers harder than loyalty members.
2. **Two sharper, localised problems sitting on top of that trend.** Wherever a discount competitor opened nearby, or wherever the recurring weekend stockout issue hit, basket value fell by roughly **18%** — nearly double the background rate. These aren't the whole story (they only touch 12 and 6 stores respectively), but they're the most fixable, highest-leverage parts of it.

---

## Data quality

- No missing values in the key numeric fields (`basket_value_zar`, `num_items`) across all 64,330 transactions.
- Blank `customer_id` values (about 55% of transactions) are expected — they mark non-loyalty shoppers, not a data problem.
- No negative or zero basket values or item counts.
- Every competitor opening and every stockout event happens to fall in the second half of 2025 — that's a real pattern in the data, not a gap in coverage (all three tables span the full year).
- All 119 stockout events are logged under a single category (Household Cleaning) and always on a Saturday or Sunday. That consistency makes for a clean, well-evidenced finding, but it's also worth flagging to FreshMart as a possible sign that other stockouts aren't being logged at all.

---

## Recommendations

| Priority | Action | Why | Owner | Risk |
|---|---|---|---|---|
| 1 | Fix the weekend delivery gap causing Household Cleaning stockouts at the 6 affected stores | Directly recovers the ~18% basket-value hit on those weekends | Store Ops / Supply Chain | Low — cause is already well understood |
| 2 | Run a targeted retention push (loyalty sign-ups, basket-building promotions) at the 12 stores with a nearby competitor, timed around the opening | Competitor-exposed stores decline at nearly double the normal rate; loyalty members already hold up better | Merchandising / Loyalty team | Medium — needs budget, and competitors may respond |
| 3 | Dig into *why* customers are shifting to smaller, more frequent trips, and adjust assortment or promotions accordingly | This is the single biggest driver, and it touches every store | Merchandising / Pricing | Medium — needs follow-up research (e.g. a customer survey) to pin down the "why" |
| 4 | Build ongoing tracking for competitor openings and stockouts, chain-wide, instead of only reviewing them after the fact | Lets FreshMart react to the next competitor opening or supply gap immediately, not a quarter later | Retail Operations | Low |

---

## Success metrics & how to test it

- **Track monthly:** average basket value, average items per basket, and the small/medium/large basket mix — chain-wide and per store.
- **Test interventions fairly:** treat stores getting a new promotion as the "treatment" group, and compare them against similar stores that aren't (same format and province, no nearby competitor) — before vs after the change. This is the same before/after logic used to isolate the competitor effect in this analysis.
- **Put a live flag on the dashboard** for any store with a newly opened nearby competitor, or an active stockout, so operations can respond as it happens rather than discovering it in next quarter's numbers.

---

## How this analysis was done, step by step

The investigation moved from simplest to most complex, one table at a time, before combining them:

1. **Profiled `Transactions` on its own first.** Row count, date coverage, and a check for missing or negative values, before comparing anything — you want to trust the data before you draw conclusions from it.
2. **Confirmed the headline concern was real.** Compared H1 vs H2 for basket value, items per basket, and transaction count, to check the "footfall up, basket down" story actually held up in the numbers.
3. **Ruled pricing in or out.** Calculated average price per item (basket value ÷ items) by month. It stayed flat, which meant the falling basket value was about *fewer items*, not *cheaper items*.
4. **Sliced `Transactions` a few more ways on its own** — by loyalty status, day of week, weekday vs weekend, and basket-value band — to see which splits showed a real difference and which didn't move at all (weekday vs weekend turned out to be a dead end).
5. **Profiled `Stores` on its own.** Store counts by province and format, and a look at which stores had a nearby competitor — this is where it became clear that every competitor opening fell in the same narrow window (August–October 2025).
6. **Profiled `Stockouts` on its own.** Found it was a small, tightly defined issue: 119 events, all one category, all at 6 stores, all on a weekend, all with the same logged cause.
7. **Joined `Transactions` to `Stores`** on `store_id` to test store-level questions: basket value by format and province, and — the key test — basket value in the weeks before vs after each store's own competitor-opening date.
8. **Joined `Transactions` to `Stockouts`** on both `store_id` and date (a stockout is specific to one store on one day, not the whole store for the year) to compare stockout days against normal days — restricted to weekend-vs-weekend, since every stockout happened on a weekend.
9. **Brought it all together.** Compared the size of each effect side by side to separate real drivers from red herrings, and built the root-cause explanation and recommendations from whichever effects were both statistically real and large enough to matter.

## Data & method notes

- **Source:** `FreshMart_Dataset.xlsx` — `Transactions` (64,330 rows), `Stores` (42 rows), `Stockouts` (119 rows), joined on `store_id`.
- **Key derived fields:** half of year (H1/H2), basket-size band, a stockout-day flag (matching `store_id` + date against the Stockouts table), and a before/after-competitor flag (transaction date vs. each store's own `competitor_open_date`).
- Full step-by-step reasoning — the business questions asked, the data-quality checks run, and the SQL behind each finding — is in [WORKFLOW.md](WORKFLOW.md).

**Tools used**

- SQL — the core analysis (see `FreshMart_Analysis.sql`)
- Excel — early exploration and prototyping
- Power BI — the executive-facing dashboard
- Python / Pandas — supporting data checks
- Databricks — hosting the reproducible workflow
- GitHub — this write-up

---

## Limitations

- This is a representative *sample* of transactions, not FreshMart's full sales ledger — good for spotting patterns and comparing segments, not for reporting exact total turnover.
- The competitor and stockout effects come from before/after comparisons, not a controlled experiment, so other things happening at the same time can't be fully ruled out.
- The shift toward smaller, more frequent trips is well supported by the data, but *why* customers are shopping this way isn't something transaction data alone can answer — that needs qualitative follow-up.

---

## Glossary

| Term | Meaning |
|---|---|
| **Basket value** | The total ZAR value of one till transaction — one shopping trip. |
| **Basket size** | How "big" a shop was, measured two ways here: basket value (ZAR) and items per basket — tracked separately because they can move in different directions. |
| **H1 / H2** | Shorthand for the first half of 2025 (Jan–Jun) and second half (Jul–Dec) — the two periods compared throughout this analysis. |
| **Footfall** | The number of shopping visits to a store, approximated here by counting transactions. |
| **Chain-wide** | True across most or all of FreshMart's 42 stores, rather than a handful of outliers. |
| **Segmentation** | Splitting the data into groups (e.g. by province, format, loyalty status) to see whether a pattern shows up everywhere equally or is concentrated in specific places. |
| **Mix-shift** | When the *proportion* of different trip types changes (e.g. more small trips, fewer large ones) — this can move the overall average even if no single customer changed their behaviour, which is why it's checked for separately from a plain average. |
| **Loyalty member / non-member** | Whether a shopper is signed up to FreshMart's loyalty programme, identified by whether a `customer_id` is recorded on the transaction. |
| **Stockout** | A recorded event where a product category was unavailable in a specific store for part of a day. |
| **Treatment vs. control** | Borrowed from experiment design: the "treatment" group is whatever was affected by something (e.g. stores that got a nearby competitor), and the "control" group is similar stores/trips that weren't — used as the baseline for comparison. |
| **Before/after window** | Comparing a store's numbers just before an event (like a competitor opening) to just after, to see whether the event coincided with a change. |
| **Root cause** | The underlying driver behind a pattern, as distinct from a surface symptom or a coincidence that happens to line up in time. |
| **ZAR** | South African Rand — the currency all basket values are measured in. |

---

## Repo structure

```
FreshMart-Basket-Analytics/
├── data/
├── excel/
├── powerbi/
├── sql/
│   └── FreshMart_Analysis.sql
├── python/
├── images/
│   └── freshmart-findings-infographic.svg
├── WORKFLOW.md
└── README.md
```
