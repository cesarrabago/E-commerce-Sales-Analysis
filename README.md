# eCommerce Sales Analysis — February 2026

> Structured EDA of **621 unique orders** across 2 markets (US, MX) and 6 product
> categories. Ships with a technical notebook and a 12-slide executive deck.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

📊 **[Full presentation (PDF, 12 slides)](Analisis_Ventas_Feb2026_CesarRabago.pdf)**

---

## The question

One month of raw order data, six channels, two currencies, and a third of the
columns mostly empty. The brief: establish what can actually be trusted, then
find what the commercial and operations teams should do about it.

---

## Headline finding

Revenue is evenly split between the two markets. Cancellations are not.

![Revenue and cancellation rate, US vs MX](images/market_us_vs_mx.png)

The US and MX markets generate near-identical revenue (~$8.4K vs $8.2K USD, a
3% gap), but **MX cancels 2.5× more often** — 10.9% against 4.4%. Whatever is
driving that gap, it is not demand.

Breaking cancellations down by payment method narrows where to look:

![Cancellation rate by payment method and fulfillment mix](images/cancellation_by_payment.png)

In MX, **CreditCard (14.2%) and Installments (12.8%)** carry roughly three times
the cancellation rate of GiftCertificate (5.0%). The same two methods in the US
sit below 5.5%. That pattern points at the payment flow rather than at logistics
or product — but see [Limitations](#limitations) before treating it as settled.

---

## Where the money is

![Estimated revenue and volume by product category](images/category_performance.png)

| Category | Revenue (USD) | Lines | Revenue per line |
|---|---:|---:|---:|
| Bundle | $6,217 | 162 | $38.4 |
| Smoked Salt | $4,885 | 245 | $19.9 |
| Seasoning | $2,256 | 149 | $15.1 |
| Sauce | $1,566 | 101 | $15.5 |
| Snack | $1,428 | 59 | $24.2 |
| Accessory | $242 | 6 | $40.3 |

**Bundle leads on revenue while ranking second on volume** — 37% of revenue from
22% of the lines. Smoked Salt moves the most units but at roughly half the
revenue per line. The two are complements, not competitors, which is what makes
the cross-sell idea below worth testing.

---

## Recommended actions

**1 · Reduce cancellations in MX.** The 10.9% rate is roughly 2.5× the US
figure. Review the payment flow first (heavy local CreditCard and Installments
use), then delivery-zone coverage and how delivery times are communicated
pre-purchase.

**2 · Treat Bundle as the revenue anchor.** It generates 37% of revenue from 22%
of lines. Cross-selling it against Smoked Salt — the volume leader — is the most
direct lever on average ticket.

**3 · Investigate the `qty = 0` lines.** 51 lines carry zero quantity but a
`Shipped` status. They may be data-entry errors, late cancellations, or
inventory adjustments; each explanation implies a different fix. This needs
operational validation, not more analysis.

**4 · Define a minimum data standard.** `tax`, `shipping_cost` and `discount`
are up to 99% null, which makes real margin and per-channel shipping ROI
impossible to compute. Fixing capture unlocks a class of analysis that is
currently closed.

---

## Key figures

| KPI | Value | Note |
|---|---|---|
| Unique orders | **621** | out of 722 total lines |
| Estimated revenue (USD) | **~$16.6K** | excludes qty=0 and lines with no price |
| MX cancellation rate | **10.9%** | vs 4.4% in the US — a 6.5 pp gap |
| Platform vs Seller fulfillment | **75% / 25%** | heavily concentrated in Platform |
| Expedited / Standard / Other shipping | **59% / 39% / 2%** | clear preference for fast shipping |
| Lines with `qty = 0` | **7.1%** | likely cancellations or adjustments |

---

## Data quality

A third of the dataset's columns are substantially empty. Rather than silently
dropping them, each gap was quantified and the handling decision recorded:

| Field | Affected | % | Handling |
|---|---:|---:|---|
| `tax` | 626 / 722 | 86.7% | Excluded from revenue — not applicable to all channels/markets |
| `shipping_cost` | 511 / 722 | 70.8% | Excluded — likely absorbed by platform fulfillment |
| `discount` | 718 / 722 | 99.4% | Ignored — only 4 lines, a fixed $71.28 one-off promotion |
| `currency` / `unit_price` | 57 / 722 | 7.9% | Excluded from revenue; retained in order counts |
| `quantity = 0` | 51 / 722 | 7.1% | Excluded from revenue, flagged for operational review |
| `city` / `country` | 3–4 / 722 | 0.4% | Retained; incomplete geolocation only |

### Analysis decisions

| # | Decision | Rationale |
|---|---|---|
| 01 | MXN → USD at a flat $20/USD | Standardizes revenue for cross-market comparison. Per-market analysis keeps the original currency. |
| 02 | Lines with `quantity = 0` excluded from revenue | 51 lines show zero quantity with a `Shipped` status — read as cancellations or inventory adjustments. |
| 03 | Volume outliers documented, not removed | 2 External-channel orders of 112 and 51 units read as B2B/wholesale. Flagged, but kept in the order count. |
| 04 | Revenue = `unit_price × quantity` | `tax` and `shipping_cost` are >70% null; including them would make revenue inconsistent across the dataset. |

---

## Limitations

Worth stating plainly, because they bound how far the findings above can be
pushed:

- **Sample size.** 621 orders over a single month. The US/MX cancellation gap
  rests on a few dozen cancelled orders per market. The direction is clear; the
  magnitude is not precise, and the payment-method breakdown splits those counts
  further still.
- **One month, no baseline.** February 2026 only. Nothing here distinguishes a
  structural pattern from a seasonal one, and the weekly trend covers five
  partial-to-full ISO weeks.
- **Correlation, not cause.** The payment-method pattern says *where* to look,
  not *why*. CreditCard users in MX may differ from GiftCertificate users in ways
  the dataset does not capture.
- **Flat exchange rate.** A single $20 MXN/USD rate was applied across the month
  rather than daily rates, which is adequate for comparison and inadequate for
  accounting.

---

## Data

<!-- TODO: replace this block with whichever applies before publishing -->
> ⚠️ *State the data's origin here — e.g. "Dataset is synthetic, generated for
> this exercise" or "Dataset is anonymized real order data, shared with
> permission."* The `ecommerce_orders_feb2026.csv` file is included / withheld
> accordingly.

---

## Deliverables

| File | What it is |
|---|---|
| `Analisis_Ventas_Feb2026_CesarRabago.pdf` | Executive presentation, 12 slides |
| `Analisis_Ventas_Feb2026.ipynb` | Full technical analysis |
| `ecommerce_orders_feb2026.csv` | Source dataset |
| `images/` | Slide exports used in this README |

## Stack

Python · pandas · matplotlib · seaborn · Jupyter

## Notebook structure

1. Loading and initial diagnosis
2. Data quality — nulls, outliers, unique values
3. Cleaning and normalization — currency conversion, filters
4. Market analysis — US vs MX
5. Category and channel analysis
6. Weekly trend — ISO weeks, Feb 2026
7. Synthesis and recommendations

---

<div align="center">

**César Rábago Pérez** · BI Analyst

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/cesar-rabago-perez)
[![GitHub](https://img.shields.io/badge/GitHub-More%20projects-181717?style=for-the-badge&logo=github)](https://github.com/cesarrabago)

</div>
