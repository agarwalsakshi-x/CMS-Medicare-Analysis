# CMS Medicare Claims Analytics Dashboard (Python, Power BI)

An end-to-end Medicare claims analytics project built on CMS synthetic claims data, which has real Medicare file structure and synthetic patient values. I cleaned 633K+ raw claim-line records in Python, converted them to one row per claim, scoped the inpatient analysis to overnight stays, and designed a 5-page interactive Power BI dashboard on Medicare spending, utilization patterns, and provider performance.

## What This Project Does

Loaded, cleaned, and analyzed 3 CMS synthetic Medicare files (Beneficiary, Inpatient, Outpatient) in Python, then built a Power BI dashboard that answers 4 business questions:

1. What is the overall volume and trend of Medicare claims?
2. What drives the highest Medicare spending?
3. What are the utilization patterns across claim types?
4. How does Medicare performance vary by geography and provider?

## Data

**Source:** CMS synthetic Medicare claims public use files, with real Medicare claims structure and synthetic patient values. The beneficiary reference year is 2025, claims span 2015–2023, and diagnoses are ICD-10 coded.

| File | Raw Rows (claim lines) | Clean Rows |
|------|------------------------|------------|
| Beneficiary Summary | 10,000 | 10,000 patients |
| Inpatient Claims | 58,066 | 20,867 claims → 5,566 overnight stays |
| Outpatient Claims | 575,092 | 402,653 claims |

## Tools

Python (Pandas, Google Colab) · Power BI Desktop 

## Data Cleaning Highlights

I inspected the raw data before writing any code and made four cleaning and scoping decisions:

- **Claim lines collapsed to claims.** The raw claim files have one row per claim line (revenue center and procedure code), with claim-level fields such as payment, dates, DRG, and provider repeated on every line. I confirmed those fields are constant within each claim, then kept one row per claim so that spend is not multiplied by the number of lines. Inpatient went from 58,066 rows to 20,867 claims, and outpatient from 575,092 rows to 402,653 claims.
- **Inpatient analysis scoped to overnight stays.** 78% of inpatient claims (16,248 of 20,867) reported 0 utilization days. I cross-checked them against admission and discharge dates. 15,301 were same-day claims (all started in the emergency room, mostly low-acuity DRGs), and I excluded them. The other 947 were actually one-night stays, so I kept them and recalculated their length of stay as 1. That leaves 5,566 overnight-stay claims. The inpatient pages describe overnight stays only.
- **Length of stay** is calculated as discharge date minus admission date. On 625 claims this is one day higher than the reported utilization-day count.
- **Dates stored as text** — converted all date columns to datetime64.
- **745 overnight-stay claims (13%) have no provider number.** They were excluded from the provider analysis only.

## Dashboard

## Overview | Claim volume trends 2015–2022, inpatient vs outpatient split

<img width="2248" height="1164" alt="image" src="https://github.com/user-attachments/assets/d804b8a9-0466-490a-9f33-60526c99c4e9" />

## Inpatient Analysis | Top DRGs by cost and LOS, scatter plot of stay vs payment

<img width="2236" height="1154" alt="image" src="https://github.com/user-attachments/assets/bdef4610-72ec-4e16-adac-de5d17138c31" />

## Outpatient Analysis | Top diagnoses by cost and volume, cost vs volume scatter

<img width="2220" height="1118" alt="image" src="https://github.com/user-attachments/assets/6d2a3013-d353-4ee8-882f-ca5595c113ba" />

## Patient Demographics | Age group distribution, spend by age, race and gender breakdown

<img width="2242" height="1262" alt="image" src="https://github.com/user-attachments/assets/594c38bc-1823-4ddc-8949-afbd7b6236a9" />

## Provider Analysis | Top providers and states by Medicare spend

<img width="2248" height="1178" alt="image" src="https://github.com/user-attachments/assets/d36d9b7d-3a8f-42f4-89f9-1656d0648c94" />

## Key Findings

Inpatient findings cover the 5,566 overnight-stay claims ($95.8M in payments).

- **DRG 180 (respiratory neoplasms) is the most expensive DRG with meaningful volume.** Its average payment is about **$106K** across 66 claims, roughly 7% of inpatient spend.
- **DRG 190 (COPD with Major Complications) has the longest average stay**, 13.4 days, and the second-highest average payment ($117K). It rests on 14 claims, so read it as a signal, not a precise estimate.
- **DRG 641 (Nutrition & Metabolism Disorders) has the highest average payment ($164K), but on only 3 claims.** I treat it as an outlier, not a spending driver.
- **Florida leads inpatient spend ($17.5M), and California leads outpatient ($81.5M).**
- **Median length of stay is 1 day and the mean is 3.95 days.** The distribution is right-skewed by complex stays. The longest is 104 days, coded DRG 951 and paid only $28K, which is far longer than typical for that DRG and worth a coding review.
- **Outpatient volume is highly concentrated in one diagnosis.** CKD Stage 4 (N184) accounts for 58.6% of outpatient claims, and one patient has 783 outpatient visits (about 2 per week). This pattern likely reflects how the synthetic data was generated.

## Limitations & Notes

- **Synthetic data.** Findings describe this dataset, not actual Medicare patterns. For example, charges equal payments on every claim, every inpatient discharge status is "home," and no deaths are recorded.
- **Scope choice.** Including same-day claims, the inpatient file has 20,867 claims ($141.0M), median length of stay 0 days, and mean 1.05 days. Florida still leads, but DRG averages shift (DRG 641 becomes 5 claims at $104.5K, and DRG 190 becomes 19 claims at $88.7K).
- **Small samples.** Average payment and length of stay for low-volume DRGs rest on few claims and should be read alongside claim counts.
- **Age.** The beneficiary file provides age as of the end of its reference year (2025), while claims span 2015–2023, so age-based spend groupings reflect current age rather than age at time of service.
- **Partial year.** 2023 is a partial year in the claims data.

## Next Steps

- Calculate age at admission for the demographics page.
- Show claim counts next to every DRG average and apply a minimum-volume threshold.
- Add a with/without same-day claims sensitivity view to the dashboard.
- Document the exact data release and download source.
