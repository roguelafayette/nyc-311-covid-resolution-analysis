# NYC 311 Service Requests: Resolution Time Analysis
### First Three Months of COVID-19 in New York City

A Python data analysis project examining how the early COVID-19 pandemic affected the resolution times of high-volume 311 service complaints across New York City. Built end-to-end from a raw BigQuery pull through cleaning, exploratory analysis, and a focused business investigation.

---

## Objective

Analyze resolution times for high-volume 311 complaint types during the first three months of confirmed COVID-19 in NYC to understand how the pandemic affected city service delivery.

**Key question:** Which complaint types took longest to resolve, and what does that reveal about city operations under crisis?

---

## Key Findings

**Two complaint types looked slow on average — but for completely different reasons.**

|    Complaint Type    |    Mean   |   Median  |                   Interpretation                     |
|----------------------|-----------|-----------|------------------------------------------------------|
| Unsanitary Condition | 35.4 days | 20.2 days | **Genuinely slow** — half of all cases took 20+ days |
| Consumer Complaint   | 15.6 days | 1.7 days  | **Outlier-skewed** — most resolved in under 2 days   |

- **Unsanitary Conditions were systemically slow.** With a median of 20 days and ~40% of cases exceeding 30 days, this reflects real delay — most likely from in-person inspections disrupted during lockdown. Sewage (46.7 days), pests (43.8 days), and mold (29.6 days) were the specific descriptors driving the backlog.
- **Consumer Complaints only *appeared* slow.** The 15.6-day average was inflated by a handful of extreme outliers (Rental Hall at 210 days, Home Heating Oil at 197 days). The median of under 2 days tells the real story.
- **The in-person vs. remote pattern held across the dataset.** Complaints requiring physical inspection lagged, while remotely-handled complaints (noise, parking, non-emergency police) resolved same-day — consistent with reduced field operations during early COVID.

**Methodological takeaway:** Reporting the mean alone would have mischaracterized consumer complaint performance. Comparing mean vs. median is essential when outliers are present.

---

## Tech Stack

- **Python** — pandas, matplotlib, seaborn
- **Google BigQuery** — public `new_york_311` dataset (~490K records after cleaning)
- **Google Colab** — analysis environment
- **SQL** — data extraction with `DATE_DIFF` resolution-time calculation

---

## Data Pipeline

1. **Extract** — SQL query against BigQuery public 311 dataset, filtered to Mar 1 – Jun 1 2020, with resolution hours calculated in-warehouse
2. **Clean** — Removed 6,535 records with negative resolution times (data errors) or resolution times exceeding one year (likely stale/lost tickets)
3. **Explore** — Complaint volume, borough distribution, resolution-time distribution
4. **Investigate** — Filtered to complaint types with 10,000+ requests, drilled into descriptors, validated averages against medians
5. **Visualize** — Purple-themed charts with data labels and outlier-aware framing

---

## Repository Structure

```
nyc-311-covid-resolution-analysis/
├── README.md
├── notebooks/
│   └── nyc_311_resolution_analysis.ipynb
├── data/
│   └── nyc_311_covid_first_3_months_cleaned.csv (zip file)
├── docs/
│   └── Executive_Summary.pdf
└── screenshots/
    ├── Consumer_complaints_outlier_distribution_chart.png
    ├── Median_vs_Mean_Chart.png
    └── Unsanitary_condition_complaint_distribution_chart.png
```

---

## How to Reproduce

1. Open the notebook in Google Colab or Jupyter
2. Authenticate to BigQuery (a Google Cloud project with the public 311 dataset enabled is required for the extraction step)
3. Run cells top-to-bottom — the cleaned CSV is also included in `/data` to skip the extraction step if preferred

---

## Caveats

- Findings are **correlational, not causal** — a pre-COVID baseline would strengthen conclusions
- Analysis covers only the first three months; longer timeframes may reveal recovery patterns
- Outlier treatment (median vs. mean) materially affects the narrative and should be considered in any operational decision

---

## About This Project

Part of a broader analytics portfolio spanning workforce/compliance, financial performance, and public/civic data. Built to demonstrate end-to-end analytical workflow: SQL extraction, data cleaning, statistical rigor (mean vs. median outlier detection), and translating findings into a defensible business narrative.

*Data source: NYC 311 Service Requests via Google BigQuery public datasets.*
