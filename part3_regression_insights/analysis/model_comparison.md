# Model Comparison — Regression Analysis of Monthly Sales

## Overview

Three regression models were built to understand the drivers of `monthly_sales` for the retail chain. The table and analysis below compare them systematically.

---

## Model Definitions

| Item | Model 1 | Model 2 | Model 3 |
|------|---------|---------|---------|
| **Model Name** | Simple Regression – Footfall | Simple Regression – Marketing Spend | Multiple Regression (Final Model) |
| **Dependent Variable** | monthly_sales | monthly_sales | monthly_sales |
| **Independent Variables** | footfall | marketing_spend | footfall, marketing_spend, inventory_availability_pct, avg_discount_pct, customer_rating, region_North, region_South, region_West |
| **N (Observations)** | 306 | 306 | 306 |

---

## R-Squared Comparison

| Metric | Model 1 | Model 2 | Model 3 |
|--------|---------|---------|---------|
| **R-Squared** | 0.7348 | 0.1574 | **0.8145** |
| **Adjusted R-Squared** | 0.7331 | 0.1519 | **0.8089** |

**Interpretation:**
- Model 1 explains **73.5%** of variation in monthly sales — strong for a single-variable model.
- Model 2 explains only **15.7%** — marketing spend alone is a weak predictor.
- Model 3 explains **81.5%** — the multiple regression model provides the best fit, capturing the combined effects of footfall, marketing, inventory, customer satisfaction, and regional factors.

---

## Coefficient Summary

### Model 1 — footfall

| Variable | Coefficient | p-Value | Significant? |
|----------|------------|---------|-------------|
| Intercept | 447,699.03 | <0.001 | Yes |
| footfall | 35.64 | <0.001 | Yes ✓ |

### Model 2 — marketing_spend

| Variable | Coefficient | p-Value | Significant? |
|----------|------------|---------|-------------|
| Intercept | 567,463.56 | <0.001 | Yes |
| marketing_spend | 2.05 | <0.001 | Yes ✓ |

### Model 3 — Multiple Regression

| Variable | Coefficient | p-Value | Significant? |
|----------|------------|---------|-------------|
| Intercept | 114,227.28 | 0.024 | Yes |
| footfall | 34.12 | <0.001 | Yes ✓ |
| marketing_spend | 1.20 | <0.001 | Yes ✓ |
| inventory_availability_pct | 2,541.14 | <0.001 | Yes ✓ |
| avg_discount_pct | -45,050.80 | 0.246 | **No** |
| customer_rating | 11,283.11 | 0.023 | Yes ✓ |
| region_North | 12,611.58 | 0.091 | Marginal |
| region_South | 23,827.80 | 0.002 | Yes ✓ |
| region_West | 19,762.80 | 0.003 | Yes ✓ |

---

## Significant Variables by Model

| Model | Significant Variables (p<0.05) |
|-------|-------------------------------|
| Model 1 | footfall |
| Model 2 | marketing_spend |
| Model 3 | footfall, marketing_spend, inventory_availability_pct, customer_rating, region_South, region_West |

---

## Business Usefulness

| Model | Business Usefulness | Reasoning |
|-------|--------------------|-----------| 
| Model 1 | High | Footfall is the single strongest predictor. Useful for quick insight but incomplete. |
| Model 2 | Moderate | Marketing spend matters but explains little variance alone. Not sufficient for decision-making by itself. |
| Model 3 | **Very High** | Captures multiple actionable drivers. Suitable for strategic planning on marketing, inventory, staffing, and regional prioritisation. |

---

## Limitations

| Model | Key Limitations |
|-------|----------------|
| Model 1 | Ignores marketing spend, inventory, customer satisfaction, and regional effects. May be spurious if footfall is driven by other factors. |
| Model 2 | Very low R² (15.7%). Marketing spend alone cannot explain sales performance well. Likely confounded by store size, location, footfall. |
| Model 3 | Still does not prove causation. Does not include store size, seasonality as a continuous variable, competitor pricing, or economic conditions. avg_discount_pct is not significant — discounting strategy's effect may be more nuanced. |

---

## Conclusion

**Model 3 (Multiple Regression)** is the superior and recommended model. It provides the highest explanatory power (R² = 0.8145) and includes multiple actionable business drivers. Leadership should use Model 3 to inform decisions on footfall generation, marketing investment, inventory management, and customer experience improvement.
