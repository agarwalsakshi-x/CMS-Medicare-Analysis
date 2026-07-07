# CMS Medicare Claims Analytics Dashboard (Python, Power BI)

Built an end-to-end Medicare claims analytics project using real CMS government data — the same structure used in production Medicare systems. Cleaned 633K+ raw records in Python, identified critical data quality issues, and designed a 5-page interactive Power BI dashboard to answer key questions about Medicare spending, utilization patterns, and provider performance.

## What This Project Does

Loaded, cleaned, and analyzed 3 CMS DE-SynPUF Medicare files (Beneficiary, Inpatient, Outpatient) in Python, then built a Power BI dashboard that answers 4 business questions:

1. What is the overall volume and trend of Medicare claims?
2. What drives the highest Medicare spending?
3. What are the utilization patterns across claim types?
4. How does Medicare performance vary by geography and provider?

## Data

**Source:** CMS DE-SynPUF (Data Entrepreneurs Synthetic Public Use File) — real Medicare data structure, synthetic patient values

| File | Raw Rows | Clean Rows |
|------|----------|------------|
| Beneficiary Summary | 10,000 | 10,000 |
| Inpatient Claims | 58,066 | 5,566 |
| Outpatient Claims | 575,092 | 402,653 |

## Tools
Python (Pandas, Google Colab) · Power BI Desktop · Notion (documentation)

## Data Cleaning Highlights

Visually inspected raw data before writing any code and identified 4 quality issues:

- **64% duplicate inpatient claims** (37,199 rows) — removed before analysis
- **78% of inpatient records were 0-day stays** — cross-checked against actual dates before removing, saving 947 legitimate 1-day stays
- **Dates stored as text** — converted all date columns to datetime64
- **745 null provider numbers** — excluded from provider analysis

## Dashboard
## Overview | Claim volume trends 2015–2022, inpatient vs outpatient split 

<img width="2264" height="1290" alt="image" src="https://github.com/user-attachments/assets/c079350a-d92c-43fb-a1d0-260228632d3a" />

 ## Inpatient Analysis | Top DRGs by cost and LOS, scatter plot of stay vs payment
 
 <img width="2286" height="1276" alt="image" src="https://github.com/user-attachments/assets/10453ca2-3750-4ed3-92e9-7f8b6511db25" />

 ## Outpatient Analysis | Top diagnoses by cost and volume, cost vs volume scatter
 
 <img width="2264" height="1282" alt="image" src="https://github.com/user-attachments/assets/87f65baa-4dd6-4266-a781-9845ac7c4ca5" />

##  Patient Demographics | Age group distribution, spend by age, race and gender breakdown

<img width="2242" height="1262" alt="image" src="https://github.com/user-attachments/assets/594c38bc-1823-4ddc-8949-afbd7b6236a9" />

 ## Provider Analysis | Top providers and states by Medicare spend 

 <img width="2260" height="1268" alt="image" src="https://github.com/user-attachments/assets/c0c5bc82-a93c-45e2-9112-978ea0463be7" />

## Key Findings

- DRG 641 (Nutrition & Metabolism Disorders) drives highest avg inpatient cost at **$164K** despite appearing non-critical
- DRG 190 (COPD with Major Complications) drives both highest LOS (13.4 days) and 2nd highest cost ($117K)
- One CKD Stage 4 patient generated **783 outpatient visits** — nearly 2 visits per week (likely dialysis)
- A 104-day stay coded as DRG 951 (typical LOS 2.6 days) was flagged as a **potential miscoding** — paid only $28K
- **Florida** leads inpatient Medicare spend ($17.5M) | **California** leads outpatient ($81.5M)
- Median LOS is 1 day — mean of 3.95 days inflated by complex outlier cases (right-skewed distribution)
