![FreshMart Basket Under Pressure — Key Findings](freshmart-findings-infographic.svg)
# Why Is FreshMart's Basket Getting Smaller?

A business analysis case study for FreshMart Supermarkets — a 42-store grocery chain across Gauteng, the Western Cape and KwaZulu-Natal. This project was completed as an individual assignment for the BrightLearn Data & AI Academy's Business Analysis programme.

## The problem I was asked to solve

More people are walking into FreshMart stores than a year ago, yet sales haven't grown to match. The Head of Retail Operations put it simply:

> "Footfall is up, but our sales aren't growing. It feels like people are buying less per visit. Find out what is going on, and tell us what to do about it."

That's a hunch, not a plan. My job was to turn it into a precise, evidence-backed answer: is basket size actually shrinking, where is it happening, why, and what should FreshMart do about it before the next budget cycle.

## What I was working with

FreshMart gave me a year of real-shaped (but sampled) operational data for 2025:

- **~64,000 till transactions** — one row per basket, with the store, date, day of week, whether the shopper was a loyalty member, how many items were in the basket, the rand value of the basket, and how they paid.
- **42 stores** — their province, format (Large / Medium / Express), floor size, how long they've been open, who manages the region, and whether a discount competitor opened nearby during the year.
- **~120 stockout events** — recorded moments where a product category ran out at a specific store, and for how long.

The three tables link together through a shared store ID, so the real work was joining them and asking the right questions — not just averaging one column.

## How I approached it

Rather than start with charts, I started by sharpening the question itself. A flat "average basket is down 3%" hides more than it reveals, so the investigation followed this path:

1. **Define the problem properly** — decide what "basket size" actually means (rand value? item count? both — they can move in opposite directions), confirm the decline is real, and put a number on it.
2. **Lay out competing explanations** — pricing, competition, product availability, changing shopping habits, store operations — and figure out what evidence in the data would prove or rule out each one.
3. **Check and clean the data** — profile it for gaps, oddities and missing values (a blank customer ID just means a non-loyalty shopper, for instance) before trusting any number that comes out of it.
4. **Dig into the segments** — split the numbers by month, store, format, province, loyalty status and day of week, because the chain-wide average is almost never where the real story lives.
5. **Separate real behaviour change from a shifting mix** — if the *kind* of shopper or trip is changing, that can look like "people spending less" without anyone actually changing their habits.
6. **Land on prioritised, costed recommendations** — with a way to measure whether they actually worked.

## What's in this repository

| File / Folder | What it is |
|---|---|
| `report/` | The full written business analysis report (problem framing, findings, recommendations) |
| `presentation/` | The boardroom-style slide deck summarising the story in 10–15 slides |
| `analysis/` | The working files behind the analysis — notebooks, SQL, or the annotated workbook, kept reproducible |
| `data/` | The FreshMart data extract (transactions, stores, stockouts) used for this investigation |

## Tools used

`[list what you actually used here — e.g. Python (pandas, matplotlib), SQL, Power BI, Excel]`

## A note on the data

This is a representative sample of FreshMart's transactions, not their full turnover — it's meant to be used for comparing patterns, not for quoting an exact company-wide sales figure. It also isn't perfectly clean, on purpose: part of the assignment was noticing and documenting the messiness, not assuming it away.

---

*This project was completed for academic purposes as part of the BrightLearn Data & AI Academy Business Analysis programme. FreshMart Supermarkets is a fictional company created for this assignment.*
