# Codebasics Virtual Internship – Promotional Campaign Analysis

## Project overview

This repository contains two analysis tasks completed as part of the Codebasics Virtual Internship. The project uses retail campaign data to examine data quality, promotional performance, sales volume, revenue, product categories, stores, and cities.

The work is organised into:

- **Task 1 – Data Cleaning and Business Analysis:** data quality checks and answers to 10 client questions.
- **Task 2 – Data Visualisation and Business Insights:** seven client questions answered with charts and business interpretations.

> **Important:** The results below are calculated from the supplied Task 1 and Task 2 CSV files. Task 1 contains 1,510 event rows before cleaning; Task 2 contains 1,500 event rows and is a separate clean input set. Do not apply Task 1 cleaning changes to Task 2 unless the client supplies a new requirement.

## Objectives

1. Validate and clean the sales event data.
2. Combine event, campaign, product, and store information using appropriate keys.
3. Compare sales quantities before and after promotions.
4. Calculate Incremental Sold Units Percentage (ISU%) and Incremental Revenue Percentage (IR%).
5. Visualise patterns and communicate actionable, evidence-based business insights.

## Tools and technologies

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Git and GitHub

## Repository structure

```text
Codebasics-Virtual-Internship/
│
├── Task_01_Data_Analysis/
│   ├── data/
│   │   ├── dim_campaigns.csv
│   │   ├── dim_products.csv
│   │   ├── dim_stores.csv
│   │   └── fact_events.csv
│   └── notebooks/
│       └── codebasics_task1_analysis.ipynb
│
├── Task_02_Data_Visualization/
│   ├── data/
│   │   ├── dim_campaigns.csv
│   │   ├── dim_products.csv
│   │   ├── dim_stores.csv
│   │   └── fact_events.csv
│   ├── notebooks/
│   │   └── codebasics_task2_visualization.ipynb
│   └── outputs/
│       └── charts/
│
├── presentation/
│   ├── client_presentation.pptx
│   └── video_presentation_script.md
│
├── README.md
└── requirements.txt
```

Adjust folder and notebook names to match the files you actually commit. Keep the Task 1 and Task 2 source CSVs in their respective folders so that the two datasets are not mixed.

## Dataset description

The project uses four CSV files:

| Dataset | Description | Main fields |
|---|---|---|
| `dim_campaigns.csv` | Campaign reference data | `campaign_id`, `campaign_name`, dates |
| `dim_products.csv` | Product reference data | `product_code`, `product_name`, `category` |
| `dim_stores.csv` | Store reference data | `store_id`, `city` |
| `fact_events.csv` | Sales and promotion event data | `event_id`, IDs, prices, quantities, `promo_type` |

The fact table connects to the dimension tables through `campaign_id`, `product_code`, and `store_id`.

## Key business metrics

### Incremental Sold Units Percentage (ISU%)

```text
ISU% = ((Quantity After - Quantity Before) / Quantity Before) * 100
```

ISU% measures the percentage change in units sold after a promotion compared with the baseline quantity before the promotion.

### Incremental Revenue Percentage (IR%)

Revenue is calculated as:

```text
Revenue = Price * Quantity
```

Then:

```text
IR% = ((Revenue After - Revenue Before) / Revenue Before) * 100
```

IR% measures the percentage change in revenue after a promotion compared with revenue before the promotion.

**Interpretation caution:** These are before-and-after comparisons. They describe observed changes and do not, by themselves, prove that the promotion caused the changes. Profitability also cannot be concluded from revenue alone because costs and margins are not included.

---

# Task 1 – Data Cleaning and Business Analysis

## Data preparation

The original Task 1 event dataset contained 1,510 rows and 9 columns.

- Found **10 duplicate rows** using the composite key `store_id`, `campaign_id`, and `product_code`.
- Removed duplicates while keeping the first occurrence, leaving **1,500 rows**.
- Found **20 missing values** in `quantity_sold(before_promo)`.
- Imputed those missing values using the median, **78**.
- Checked the cleaned data after the operations.

The median was used because it is less sensitive to extreme values than the mean.

## Client questions and findings

| Q | Client question | Finding |
|---|---|---|
| 1 | How many duplicate event rows were removed? | 10 duplicate rows removed; 1,500 rows remained. |
| 2 | How many cities have more than five stores? | 3 cities: Bengaluru (10), Chennai (8), Hyderabad (7). |
| 3 | How many missing pre-promotion quantities were filled, and what median was used? | 20 values filled using median 78. |
| 4 | Which product category has the lowest pre-promotion base price? | Personal Care; minimum base price 50 (dataset price units). |
| 5 | What was the total post-promotion quantity for BOGOF during Diwali? | 34,461 units. |
| 6 | Which store had the highest post-promotion quantity during Diwali? | STCHE-4, with 5,013 units. |
| 7 | Which campaign had the greater increase in units sold? | Sankranti: increase of 154,175 units; Diwali: increase of 73,648 units. |
| 8 | Which product had the highest IR% during Sankranti? | Product code P03, `Atliq_Suflower_Oil (1L)` as spelled in the supplied product file; IR% approximately 91.83%. |
| 9 | Which Visakhapatnam store had the lowest ISU% during Diwali? | STVSK-3; ISU% approximately 49.21%. |
| 10 | Which Sankranti promotion type had both negative IR% and negative ISU%? | 25% OFF; ISU% approximately -19.60% and IR% approximately -39.33%. |

### Task 1 takeaway

The data cleaning steps improved the reliability of the analysis. Sankranti had a larger absolute increase in units sold than Diwali in the supplied data. The 25% OFF promotion was the only Sankranti promotion type with both negative ISU% and negative IR%.

---

# Task 2 – Data Visualisation and Business Insights

Task 2 uses a separate 1,500-row event dataset. Its supplied `quantity_sold(before_promo)` column has no missing values and no duplicate composite keys in the supplied file.

## Client questions, visualisations, and findings

| Q | Analysis | Visualisation | Finding |
|---|---|---|---|
| 1 | Number of stores in each city | Bar chart | Bengaluru has 10 stores, Chennai 8, and Hyderabad 7. |
| 2 | Sankranti post-promotion quantity contribution by category | Pie chart | Grocery & Staples contributes 70.51% of 252,069 units. |
| 3 | Correlation between post-promotion price and quantity | Heatmap | Pearson correlation is approximately +0.2694, a weak positive linear relationship. |
| 4 | Distribution of pre-promotion quantity by category | Individual histograms | Grocery & Staples has the highest mean (314.79) and median (317); Home Care has the lowest mean (49.58) and median (43). |
| 5 | City-wise ISU% | Line chart | Madurai is highest at 121.28%; Visakhapatnam is lowest at 99.07%. |
| 6 | IR% and ISU% by promotion type in Hyderabad | Scatter plot | BOGOF has the highest ISU% (252.83%); 500 Cashback has the highest IR% (122.82%). |
| 7 | Bengaluru revenue before and after promotion by category | Grouped vertical bar chart | Overall revenue increased by approximately 81.34%; Personal Care revenue decreased by approximately 32.39%. |

### Task 2 category contribution – Sankranti

| Category | Units after promotion | Contribution |
|---|---:|---:|
| Grocery & Staples | 177,724 | 70.51% |
| Home Appliances | 35,610 | 14.13% |
| Home Care | 16,894 | 6.70% |
| Combo1 | 12,411 | 4.92% |
| Personal Care | 9,430 | 3.74% |
| **Total** | **252,069** | **100%** |

### Task 2 Hyderabad promotion comparison

| Promotion type | ISU% | IR% |
|---|---:|---:|
| BOGOF | 252.83% | 74.12% |
| 500 Cashback | 167.38% | 122.82% |
| 33% OFF | 45.17% | -2.98% |
| 50% OFF | 35.94% | -31.99% |
| 25% OFF | -12.90% | -34.77% |

### Task 2 Bengaluru revenue comparison

| Category | Revenue before | Revenue after | Change |
|---|---:|---:|---:|
| Combo1 | 15,777,000 | 38,125,000 | +141.65% |
| Grocery & Staples | 12,262,460 | 13,861,259 | +13.04% |
| Home Appliances | 2,188,810 | 4,122,680 | +88.35% |
| Home Care | 2,104,460 | 3,180,020 | +51.11% |
| Personal Care | 576,321 | 389,629 | -32.39% |

Overall revenue before promotion was 32,909,051 and after promotion was 59,678,568, an increase of approximately 81.34%.

### Task 2 takeaway

The analysis shows that promotional outcomes vary by category, city, and promotion type. Grocery & Staples dominates Sankranti unit sales contribution. In Hyderabad, BOGOF produces the largest increase in units, while 500 Cashback produces the largest revenue percentage increase. In Bengaluru, overall revenue rises, but Personal Care declines.

---

## Overall recommendations

1. Evaluate promotions using both ISU% and IR%, rather than sales volume alone.
2. Investigate why Personal Care revenue declined in Bengaluru.
3. Review the strong unit uplift from BOGOF and the revenue uplift from 500 Cashback in Hyderabad.
4. Study city-level differences before applying the same promotion strategy everywhere.
5. Include profit margin, discount cost, and a suitable control group in future analysis before making causal or profitability claims.

## How to run the project

1. Clone or download this repository.
2. Create and activate a Python virtual environment.
3. Install the dependencies.
4. Open the relevant notebook in Jupyter or VS Code.
5. Confirm that each notebook points to its own task-specific `data/` folder.
6. Run all notebook cells from top to bottom.

Example setup:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Requirements

Create a `requirements.txt` file with:

```text
pandas
numpy
matplotlib
seaborn
jupyter
ipykernel
```

## Limitations

- The analysis uses the supplied before-and-after data and does not establish causation.
- Revenue is not profit. Cost, margin, inventory, and discount expense data are not available here.
- Correlation measures linear association and does not establish a causal relationship.
- The project findings are limited to the provided datasets and campaign records.

## Author

**Sonalika Singh**  
Mathematics postgraduate | Aspiring Data Analyst / Data Scientist


## Acknowledgement

Project tasks and datasets are from the Codebasics Virtual Internship. This repository documents my independent analysis, learning, and presentation of the supplied business questions.
