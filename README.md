# European-Real-Estate
Here is the competition-ready Power BI-grade dashboard built directly from your actual EU_Real_Estate_Dataset.xlsx using AI
# 🏢 European Real Estate Market Intelligence Dashboard
### Power BI Competition Entry — Data Analytics Challenge 2024

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Excel](https://img.shields.io/badge/Microsoft%20Excel-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Status](https://img.shields.io/badge/Status-Competition%20Ready-brightgreen?style=for-the-badge)

---

## 📌 Project Overview

A **competition-grade Power BI dashboard** analysing 5,000 European property listings across 10 countries, 10 cities, and 7 property types. Built for real estate analysts, property investors, and marketplace product teams to understand market trends, compare locations, and evaluate property value drivers.

> **Dataset**: `EU_Real_Estate_Dataset.xlsx` — 5,000 rows × 25 columns  
> **Challenge**: European Real Estate Market Analytics — Data Visualisation Competition 2024

---

## 🗂️ Repository Structure

```
eu-realestate-pbi/
│
├── 📁 data/
│   ├── EU_Real_Estate_Dataset.xlsx        ← Raw source dataset (25 columns, 5,000 rows)
│   └── data_dictionary.md                 ← Column definitions, types, and notes
│
├── 📁 dax/
│   ├── 01_core_kpis.dax                   ← Total Listings, Avg Price, Avg DOM, Yield
│   ├── 02_running_totals.dax              ← RT by Country, City, Property Type
│   ├── 03_time_intelligence.dax           ← YTD, MoM%, QoQ%, YoY Growth
│   ├── 04_price_analysis.dax              ← Appreciation %, Premium vs Market, Amenity lift
│   ├── 05_investment_measures.dax         ← Gross Yield, Investment Score, CAGR
│   ├── 06_market_dynamics.dax             ← Illiquid %, Market Share, High-Val/Low-Activity
│   └── 07_segmentation.dax               ← Energy Score, Age Band, Commercial vs Residential
│
├── 📁 dashboard/
│   ├── EU_RealEstate_Dashboard.pbix       ← Main Power BI file
│   └── EU_RealEstate_Dashboard.html       ← Interactive HTML preview (no Power BI needed)
│
├── 📁 docs/
│   ├── data_dictionary.md                 ← Full column reference
│   ├── dax_reference.md                   ← All 24+ measures with explanations
│   ├── visual_design_guide.md             ← Theme, colours, layout decisions
│   └── insights_report.md                 ← Key findings for judges/stakeholders
│
├── 📁 assets/
│   ├── screenshots/
│   │   ├── dashboard_overview.png
│   │   ├── kpi_strip.png
│   │   ├── country_analysis.png
│   │   └── investment_matrix.png
│   └── theme/
│       └── eu_realestate_theme.json       ← Power BI custom colour theme file
│
├── .gitignore
├── LICENSE
└── README.md                              ← You are here
```

---

## 📊 Dataset Overview

| Attribute | Value |
|---|---|
| **Source file** | `EU_Real_Estate_Dataset.xlsx` |
| **Total listings** | 5,000 |
| **Distinct countries** | 10 |
| **Distinct cities** | 10 |
| **Distinct property types** | 7 |
| **Listing types** | Sale (2,955) · Rental (2,045) |
| **Date range** | 2020 Q1 → 2024 Q4 |
| **Columns** | 25 |

### Countries & Cities
| Country | City | Listings (Sale) | Avg €/m² |
|---|---|---|---|
| France | Paris | 429 | €8,637 |
| Netherlands | Amsterdam | 346 | €7,920 |
| Germany | Berlin | 410 | €4,954 |
| Sweden | Stockholm | 303 | €4,716 |
| Belgium | Brussels | 178 | €4,613 |
| Italy | Rome | 324 | €4,461 |
| Spain | Madrid | 309 | €3,768 |
| Czech Republic | Prague | 195 | €3,585 |
| Portugal | Lisbon | 235 | €3,138 |
| Poland | Warsaw | 226 | €2,394 |

### Property Types
| Type | Listings | Avg Price | Avg €/m² |
|---|---|---|---|
| Apartment | 2,057 | €575,937 | €2,789 |
| Villa | 679 | €752,181 | €3,282 |
| Office | 620 | €1,885,570 | €3,652 |
| Townhouse | 489 | €578,708 | €3,042 |
| Retail | 541 | €1,858,351 | €3,388 |
| Mixed-Use | 368 | €1,845,234 | €3,781 |
| Warehouse | 246 | €1,672,534 | €3,322 |

---

## ⚡ Key DAX Measures — Quick Reference

### DISTINCTCOUNT Measures (Start Here)
```dax
-- Number of distinct countries
Distinct Countries = DISTINCTCOUNT( EU_Real_Estate[country] )

-- Number of distinct cities
Distinct Cities = DISTINCTCOUNT( EU_Real_Estate[city] )

-- Number of distinct property types
Distinct Property Types = DISTINCTCOUNT( EU_Real_Estate[property_type] )
```

### Core KPIs
```dax
Total Listings       = COUNTROWS( EU_Real_Estate )
Avg Sale Price       = CALCULATE( AVERAGE( EU_Real_Estate[sale_price_eur] ), EU_Real_Estate[listing_type] = "Sale" )
Avg Price per m²     = CALCULATE( AVERAGE( EU_Real_Estate[price_per_sqm] ), EU_Real_Estate[listing_type] = "Sale" )
Avg Days on Market   = CALCULATE( AVERAGE( EU_Real_Estate[days_on_market] ), NOT ISBLANK( EU_Real_Estate[days_on_market] ) )
Gross Yield %        = DIVIDE( CALCULATE( AVERAGE( EU_Real_Estate[monthly_rent_eur] ), EU_Real_Estate[listing_type] = "Rental" ) * 12, CALCULATE( AVERAGE( EU_Real_Estate[sale_price_eur] ), EU_Real_Estate[listing_type] = "Sale" ), BLANK() )
```

### Running Totals — Country
```dax
RT Sale Price by Country =
VAR CurrentCountry = MAX( EU_Real_Estate[country] )
RETURN CALCULATE(
    SUM( EU_Real_Estate[sale_price_eur] ),
    FILTER( ALL( EU_Real_Estate[country] ), EU_Real_Estate[country] <= CurrentCountry ),
    EU_Real_Estate[listing_type] = "Sale"
)
```

### Running Totals — City
```dax
RT Sale Price by City =
VAR CurrentCity = MAX( EU_Real_Estate[city] )
RETURN CALCULATE(
    SUM( EU_Real_Estate[sale_price_eur] ),
    FILTER( ALL( EU_Real_Estate[city] ), EU_Real_Estate[city] <= CurrentCity ),
    EU_Real_Estate[listing_type] = "Sale"
)
```

### Running Totals — Property Type
```dax
RT Sale Price by Type =
VAR CurrentType = MAX( EU_Real_Estate[property_type] )
RETURN CALCULATE(
    SUM( EU_Real_Estate[sale_price_eur] ),
    FILTER( ALL( EU_Real_Estate[property_type] ), EU_Real_Estate[property_type] <= CurrentType ),
    EU_Real_Estate[listing_type] = "Sale"
)
```

> See the full `/dax/` folder for all 24+ measures with explanations.

---

## 📈 Dashboard Visuals

| # | Visual | Type | Dimension | Measure |
|---|---|---|---|---|
| 1 | Price/m² by Country | Horizontal Bar | Country | Avg Price per m² |
| 2 | Listing Type Split | Donut Chart | Listing Type | Total Listings |
| 3 | Property Type Mix | Combo Bar+Line | Property Type | Count + Avg €/m² |
| 4 | Gross Yield by Country | Bar Chart | Country | Gross Yield % |
| 5 | Quarterly Price Trend | Line Chart | Quarter | Avg Price/m² (Sale vs Rent) |
| 6 | Price Appreciation | Bar Chart | Country | Price Appreciation % |
| 7 | Amenity Premium | Signed Bar | Amenity | Price Premium % |
| 8 | Days on Market Dist. | Histogram | DOM Band | Listing Count |
| 9 | Energy Rating Dist. | Bar Chart | Rating (A–G) | Listing Count |
| 10 | Investment Scorecard | Table | City | Composite Score |
| 11 | Bedroom vs €/m² | Bar Chart | Bedrooms | Avg Price per m² |
| 12 | Price vs Size Scatter | Scatter Plot | Square Meters | Sale Price |
| 13 | Furnishing Premium | Bar Chart | Furnishing Status | Avg Price per m² |

---

## 🔍 Key Insights

| Finding | Detail |
|---|---|
| **Paris leads pricing** | €8,637/m² — 72% above EU average of €5,187 |
| **Austria tops yield** | 5.20% gross rental yield — best investor return |
| **Market is illiquid** | 43% of listings sit 180–365 days on market |
| **Parking premium** | +11.8% price lift vs listings without parking |
| **Elevator premium** | +10.8% — second strongest amenity driver |
| **Pool is flat** | −2.9% — concentrated in lower-density villa stock |
| **Energy: mostly D–E** | 49% rated D or E · only 2.6% (130 listings) achieve A |
| **Belgium tops growth** | +117% vs last sold price — highest capital gain |
| **Apartments dominate** | 41% of all listings — but commercial types earn more |
| **Furnishing negligible** | Only €42/m² gap between furnished and unfurnished |

---

## 🚀 Getting Started

### Option A — Power BI Desktop
1. Clone this repository
```bash
git clone https://github.com/YOUR_USERNAME/eu-realestate-pbi.git
cd eu-realestate-pbi
```
2. Open `dashboard/EU_RealEstate_Dashboard.pbix` in Power BI Desktop
3. If data refresh fails, update the file path in **Transform Data → Source**
4. Import DAX measures from `/dax/` folder into your measure tables

### Option B — HTML Preview (No Power BI required)
```bash
# Just open in any browser
open dashboard/EU_RealEstate_Dashboard.html
```

### Option C — Rebuild from scratch
1. Load `data/EU_Real_Estate_Dataset.xlsx` into Power BI
2. Apply Power Query transformations (see `docs/data_dictionary.md`)
3. Paste DAX measures from `/dax/*.dax` files
4. Apply theme from `assets/theme/eu_realestate_theme.json`

---

## 🎨 Theme & Design

| Element | Value |
|---|---|
| **Primary colour** | `#F5C542` (Competition Gold) |
| **Secondary** | `#4A90D9` (Steel Blue) |
| **Accent** | `#2EC4B6` (Teal) |
| **Danger/Alert** | `#E05C5C` (Red) |
| **Success/Yield** | `#5CBF8A` (Green) |
| **Background** | `#0B0F19` (Dark Navy) |
| **Card surface** | `#131929` |
| **Font** | DM Mono / DM Sans |
| **Chart library** | Chart.js v4.4 (HTML preview) |

---

## 📁 DAX Files Summary

| File | Measures | Description |
|---|---|---|
| `01_core_kpis.dax` | 6 | Total Listings, Avg Price, Avg €/m², DOM, Yield, Premium % |
| `02_running_totals.dax` | 9 | RT by Country (4), RT by City (4), RT by Type (3) |
| `03_time_intelligence.dax` | 4 | YTD, MoM%, QoQ%, YoY Growth |
| `04_price_analysis.dax` | 4 | Appreciation %, Premium vs Market, Amenity lifts, Band Concentration |
| `05_investment_measures.dax` | 4 | Gross Yield, Investment Score, CAGR, Rent-to-Price |
| `06_market_dynamics.dax` | 4 | Illiquid %, Market Share, High-Val/Low-Activity, Parking Rate |
| `07_segmentation.dax` | 3 | Energy Score, Age Band, Commercial Premium |
| **Total** | **34** | |

---

## 🙋 Author

**Albert Better Ikiyoudoutei**  
Founder & CEO, AgriX · Head of Accounting, Biokeft Energies Limited  
B.Sc. Economics — Delta State University  
Certifications: ALX · Microsoft · IBM · HP LIFE · Chartered Manager  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://linkedin.com)

---

## 📄 License

```
MIT License — free to use, modify, and distribute with attribution.
Dataset is synthetic and generated for competition purposes only.
```

---

> *Built for the European Real Estate Market Analytics Challenge 2024.*  
> *All insights derived from the provided EU_Real_Estate_Dataset.xlsx.*
