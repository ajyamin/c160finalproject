# Obesity Paradox in the ICU: A Causal Analysis of Effect Heterogeneity

**UCLA STATS C160 Final Project**  
**Allen Yamin | March 2026**

## Overview

This project investigates the "obesity paradox" in critical care: the observation that overweight or obese patients may experience lower mortality than normal-weight patients in some ICU populations.

Using **MIMIC-IV electronic health record data from 21,766 ICU admissions**, I applied causal inference and machine learning methods to estimate the effect of elevated BMI (BMI ≥ 25) on in-hospital mortality and examine whether that effect varies across patient subgroups.

The primary research question was:

> **Does the causal effect of elevated BMI on ICU mortality vary across subpopulations, particularly by sex and admission type?**

## Methods

The analysis uses a progression of causal estimators to evaluate how the estimated effect changes with increasingly robust adjustment for confounding:

- Naive treatment-value comparison
- Inverse Probability Weighting (IPW)
- Logistic outcome regression
- Augmented Inverse Probability Weighting (AIPW)
- Cross-fitted AIPW / Double Machine Learning (DML)
- Causal forests for exploratory heterogeneous treatment effect estimation

A directed acyclic graph (DAG) was used to define the assumed causal structure and adjustment set. Age, sex, and comorbidity burden were treated as primary confounders.

Machine-learning nuisance models were estimated using **XGBoost**, with cross-fitting used to reduce overfitting bias.

## Data

The analysis uses de-identified ICU data from **MIMIC-IV**, accessed through the `ricu` R package.

Final analytic cohort:

- **21,766** adult ICU patients
- **18,747** medical admissions
- **3,019** surgical admissions
- **7.0%** overall in-hospital mortality
- **60.4%** with BMI ≥ 25

### Variables

- **Treatment:** Elevated BMI (BMI ≥ 25)
- **Outcome:** In-hospital mortality
- **Adjustment covariates:** Age, sex, comorbidity proxy
- **Primary effect modifier:** Sex
- **Secondary effect modifier:** Admission type

## Key Results

After adjustment using doubly robust methods, the overall estimated effect of elevated BMI on ICU mortality was small and statistically non-significant:

**DML ATE = -0.004 (p = 0.24)**

However, the analysis identified statistically significant effect heterogeneity by sex:

| Subgroup | Estimated Effect | p-value |
|---|---:|---:|
| Female | -0.0141 | 0.009 |
| Male | +0.0043 | 0.349 |
| Female - Male difference | -0.0183 | 0.009 |

The estimated protective association was concentrated among female patients, while the estimated effect among male patients was near zero.

No evidence of effect modification by medical versus surgical admission type was found.

An exploratory causal forest analysis also provided evidence of heterogeneous treatment effects, with sex emerging as the primary observed driver of variation.

## Sensitivity Analysis

Two robustness analyses were performed:

1. **E-values** to assess sensitivity to unmeasured confounding.
2. Re-estimation using the stricter clinical obesity threshold of **BMI ≥ 30**.

The sex-based heterogeneity persisted under the BMI ≥ 30 definition (sex-difference p = 0.010).

## Limitations

Important limitations include:

- Use of an ICD-code count as a proxy for Charlson comorbidity burden
- Potential BMI measurement misclassification
- Lack of SOFA severity scores in the extracted dataset
- Remaining potential for unmeasured confounding
- Limitations of uncertainty estimates from the exploratory causal forest analysis

Because this is an observational analysis, the causal interpretation depends on the stated identification assumptions.

## Tools & Techniques

**Language:** R

**Methods:**  
Causal inference · Double machine learning · AIPW · IPW · Logistic regression · XGBoost · Causal forests · Cross-fitting · Sensitivity analysis · Effect heterogeneity

**Data:**  
MIMIC-IV electronic health records

## Project Files

- [`c160_analysis1.Rmd`](c160_analysis1.Rmd) — Full R analysis, including data preparation, causal estimation, machine learning models, sensitivity analyses, and visualizations.
- [`Obesity_Paradox_ICU_Report.pdf`](Obesity_Paradox_ICU_Report.pdf) — Final research report describing the causal question, methodology, results, assumptions, and limitations.

---

*UCLA Department of Statistics & Data Science — STATS C160*
