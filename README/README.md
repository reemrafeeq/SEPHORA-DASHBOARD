# Sephora Collection - Sales Analysis (Power BI)

An interactive Power BI dashboard that analyses Sephora Collection sales: revenue, profit, trends, products, regions, discounts and customer behaviour.

> **Note:** Public Sephora datasets do not contain transactions, so the sales data in this project is **simulated** with a reproducible script (`Scripts/generate_dataset.py`). The findings demonstrate the analysis method and dashboard, not real company performance.

## Deliverables

| Deliverable | Location | Status |
|---|---|---|
| BRD | `Docs/BRD.md` and `Docs/PDF/BRD.pdf` | Done |
| FRD | `Docs/FRD.md` and `Docs/PDF/FRD.pdf` | Done |
| KPI definitions | `Docs/KPI_Definitions.md` and `Docs/PDF/KPI_Definitions.pdf` | Done |
| Dataset assessment | `Docs/Dataset_Assessment.md` and `Docs/PDF/Dataset_Assessment.pdf` | Done |
| Data quality checks | `Docs/Data_Quality_Report.md` and `Docs/PDF/Data_Quality_Report.pdf` (43 of 43 passed) and `Scripts/data_quality_checks.py` | Done |
| Cleaned dataset | `Data/Cleaned/` | Done |
| Dashboard mockup | `Dashboard/Mockup/` (4 PNGs and a PDF) | Done |
| Power BI file | `Dashboard/SephoraSalesAnalysis.pbix` | **Add your own file** |
| Dashboard export | `Dashboard/Export/` | **Export from Power BI** |
| Analysis report | `Reports/Analysis_Report.pdf` | Done |
| README | `README.md` | Done |

## Project overview

The business wants to know which products, categories and regions drive sales, how revenue and profit change over time, and how discounts and customer behaviour affect results. This project delivers a 3-page dashboard (plus a drill-through page), a written analysis report and the supporting data and documentation.

**Business questions**
- Which products and categories earn the most revenue and profit?
- How do revenue and profit change by month and year?
- How do cities and regions compare?
- How do discounts affect profit margin?
- What do customers buy, and how often?

## Dataset summary

Star schema, 2022-2025, currency INR.

| Table | Rows | Role |
|---|---:|---|
| `Sales` | 106,987 | Fact: one row per product on an order |
| `Products` | 184 | Dimension: Sephora Collection products, 7 categories |
| `Customers` | 20,000 | Dimension: gender, age group, segment |
| `Geography` | 30 | Dimension: city, state, region |
| `Calendar` | 1,461 | Date dimension |

60,000 orders and 17,940 customers who bought. See `Docs/Data_Dictionary.md` for every column.

## Cleaning summary

- Promoted headers, renamed queries and set data types in Power Query.
- Checked data quality: 0 nulls, 0 duplicate rows, unique `OrderLineID`.
- Validated that `Profit = Revenue - TotalCost` on every row.
- Reconciled totals against the source files.
- Built four many-to-one relationships, marked `Calendar` as the date table and sorted `MonthName` by `MonthNumber`.

## KPI definitions

| KPI | Definition | Value |
|---|---|---:|
| Total Revenue | Sum of revenue after discount | INR 180.9M |
| Total Profit | Revenue minus cost | INR 85.0M |
| Profit Margin % | Profit / Revenue | 47.0% |
| Total Units Sold | Sum of quantity | 165,584 |
| Total Orders | Distinct order IDs | 60,000 |
| Distinct Customers | Distinct customer IDs | 17,940 |
| Average Order Value | Revenue / Orders | INR 3,015 |
| Average Selling Price | Revenue / Units | INR 1,092 |
| Total Discount | Sum of discount amount | INR 19.9M |
| Revenue Growth % | (Revenue - Revenue LY) / Revenue LY | +13.7% (2025) |

DAX for all measures is in `Docs/DAX_Measures.md`.

## Insights

- **Growth is steady but slowing:** revenue grew from INR 34.1M (2022) to INR 55.7M (2025); yearly growth fell from 23.3% to 16.5% to 13.7%.
- **Two categories carry the business:** Makeup (52.5%) and Skincare (29.9%) earn 82.3% of revenue.
- **South leads (36.4%)**; East and Central together give only 13.4%. The top 5 cities produce 43.8% of revenue.
- **Q4 is the peak:** October to December bring 30.5% of annual revenue.
- **Deep discounts hurt margin:** margin falls from 52.3% (no discount) to 34.0% (21-30% off).
- **Loyal customers matter:** 77.5% of customers ordered more than once; VIPs are 13.6% of customers but 28.9% of revenue.

Full discussion with charts: `Reports/Analysis_Report.pdf`.

## Recommendations

1. Protect Makeup and Skincare, and promote hero products in Face and Eyes.
2. Prepare stock and campaigns for Q4, and lift the slow months (January, February).
3. Keep most discounts at or below 20%; prefer bundles and loyalty points to deep cuts.
4. Test online campaigns in East and Central regions.
5. Reward VIP and Loyalty customers and move Regular customers up.
6. Review low-margin items such as Nails and Bath and Body.

## Dashboard overview

![Overview mockup](Dashboard/Mockup/01_Overview.png)

*Design mockup drawn from the real data. Replace it with a Power BI screenshot from `Screenshots/` when you have one.*

| Page | Content |
|---|---|
| Overview | 6 KPI cards, revenue and profit trend, category donut, top products, revenue by region |
| Products | 4 KPI cards, revenue by category, sub-category treemap, product table, top 5 and bottom 5 |
| Regions | 4 KPI cards, bubble map, top cities, region by category matrix |
| Product Details (hidden) | Drill-through from the product table |

Features: Sephora theme, header page navigator, synced slicers (Year, Region, Category, Channel), conditional formatting and drill-through. Screenshots are in `Screenshots/`.

## Limitations

- The data is simulated, so patterns (seasonality, regional shares, discount behaviour) come from the generator settings.
- Cost is a fixed share of unit price; returns, refunds, shipping and stock-outs are not modelled.
- No forecasting or real-time monitoring (out of scope).
- Findings show associations, not proven causes.

## Repository structure

```
Sephora_PowerBI_Project/
├── README.md
├── .gitignore
├── requirements.txt
├── Data/
│   ├── Raw/                  Source CSVs (simulated)
│   └── Cleaned/              Validated CSVs used by Power BI
├── Dashboard/
│   ├── Mockup/               Design mockups (PNG and PDF)
│   ├── Export/               Put the Power BI PDF export here
│   └── SephoraSalesAnalysis.pbix   (add your file)
├── Docs/
│   ├── BRD.md
│   ├── FRD.md
│   ├── KPI_Definitions.md
│   ├── Dataset_Assessment.md
│   ├── Data_Quality_Report.md
│   ├── PDF/                  PDF copies of the documents
│   ├── DAX_Measures.md
│   └── Data_Dictionary.md
├── Reports/
│   ├── Analysis_Report.pdf
│   └── figures/
├── Scripts/
│   ├── generate_dataset.py
│   ├── data_quality_checks.py
│   ├── build_mockups.py
│   ├── build_doc_pdfs.py
│   └── build_report.py
├── Screenshots/              Real Power BI screenshots
└── Theme/
    ├── Sephora_Theme.json
    └── Sephora_Background_1280x720.png
```

## How to use

1. Open Power BI Desktop, load the five CSVs from `Data/Cleaned/`, and create the relationships described in `Docs/Data_Dictionary.md`.
2. Import `Theme/Sephora_Theme.json` (View, Themes, Browse for themes) and set `Theme/Sephora_Background_1280x720.png` as the canvas background.
3. Add the measures from `Docs/DAX_Measures.md`.
4. Optional: run the scripts in order from the `Scripts/` folder (`pip install -r requirements.txt`): `generate_dataset.py`, `data_quality_checks.py`, `build_mockups.py`, `build_report.py`.

## Tools

Power BI Desktop, DAX, Power Query, Python (pandas, numpy, matplotlib, Pillow, reportlab).

## Author

Your name - add your LinkedIn or GitHub link here.
