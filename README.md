# eCommerce Sales Analysis — February 2026

> A structured EDA covering **621 unique orders** across 2 markets (US, MX) and 6 product categories. Includes a technical Python notebook and an executive PowerPoint presentation.

---

## Context

Exploratory analysis of a February 2026 ecommerce order dataset, focused on four fronts: **data quality, commercial health, market-level behavior, and operational optimization opportunities**. The final deliverable is an executive presentation aimed at commercial and operations teams.

## Objectives

- Diagnose the dataset's quality and document every cleaning decision
- Compute baseline KPIs for revenue, cancellations, and fulfillment type
- Compare behavior across markets (US vs MX)
- Generate actionable insights for commercial and operations teams

## Data quality decisions

| # | Decision | Rationale |
|---|---|---|
| 01 | MXN → USD conversion ($20 MXN/USD) | Standardize revenue for cross-market comparison; per-market analysis keeps the original currency |
| 02 | Exclusion of lines with `quantity = 0` | 51 lines with "Shipped" status interpreted as cancellations/inventory adjustments |
| 03 | Documentation of outliers in the External channel | 2 orders with quantities of 112 and 51 units identified as B2B/wholesale, kept with a flag |
| 04 | Revenue computed as `unit_price × quantity` | The `tax` and `shipping_cost` fields are >70% null; omitted to maintain consistency |

## Key insights

| KPI | Value | Comment |
|---|---|---|
| Unique orders | **621** | Out of 722 total lines |
| Estimated revenue (USD) | **~$16.6K** | Excludes lines with qty=0 and lines without a price |
| MX cancellation rate | **10.9%** | vs 4.4% in the US — a **6.5 pp** gap |
| Platform vs Seller fulfillment | **75% / 25%** | Heavy concentration in Platform |
| Expedited vs Standard shipping | **59% / 39%** | Clear preference for fast shipping |
| Lines with `qty=0` | **7.1%** | Likely cancellations/adjustments |

**Main finding:** The MX market shows a cancellation rate 2.5x higher than the US. This points to fulfillment, pre-sale communication, or inventory management issues specific to the Mexican market that warrant deeper investigation.

## Deliverables

| File | Type |
|---|---|
| `Analisis_Ventas_Feb2026_CesarRabago.pptx` | Executive presentation (12 slides) |
| `Analisis_Ventas_Feb2026.ipynb` | Notebook with the full technical analysis |
| `ecommerce_orders_feb2026.csv` | Dataset (if data policies allow) |

## Stack

Python · pandas · matplotlib · seaborn · PowerPoint

## Notebook structure

1. Loading and initial diagnosis
2. Data quality (nulls, outliers, unique values)
3. Cleaning and normalization (currency conversion, filters)
4. Market analysis (US vs MX)
5. Category and channel analysis
6. Weekly trend (ISO week, Feb 2026)
7. Synthesis and recommendations
