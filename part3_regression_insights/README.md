# Regression-Based Business Insights — Retail Chain Sales Analysis

## Repository Structure

```
part3_regression_insights/
├── data/
│   └── business_regression_data.xlsx
├── analysis/
│   ├── regression_workbook.xlsx
│   ├── model_comparison.md
│   └── residual_analysis.md
├── outputs/
│   ├── regression_summary.xlsx
│   ├── final_recommendation.md
│   └── model_equations.md
├── screenshots/
│   ├── simple_regression_output.png
│   ├── multiple_regression_output.png
│   ├── residuals_preview.png
│   └── model_comparison_preview.png
└── README.md
```

---

## Business Problem Summary

The leadership team of a retail chain wants to understand what factors are driving monthly sales performance across stores. Decisions under consideration include:
- Increasing or reallocating marketing spend
- Improving inventory availability
- Changing discount strategies
- Improving customer experience
- Prioritising certain regions or store types

This analysis uses **regression analysis** to identify which factors are most strongly associated with monthly sales and provides evidence-based recommendations.

---

## Dataset Description

**File:** `data/business_regression_data.xlsx`  
**Sheet:** `store_performance`  
**Records:** 320 store-month observations  
**Stores:** 80 unique stores  
**Time Period:** January – April 2025 (4 months)  
**Records used in analysis:** 306 (14 excluded due to missing values in `customer_rating` and `competitor_distance_km`)

### Variables

| Variable | Type | Role | Description |
|----------|------|------|-------------|
| store_id | Categorical | Identifier | Unique store code |
| month | Date | Identifier | Month of observation |
| region | Categorical | Independent | Store region (East, North, South, West) |
| store_type | Categorical | Independent | Store format (Residential, High Street, Airport, Mall) |
| marketing_spend | Numerical | Independent | Monthly marketing expenditure (£) |
| footfall | Numerical | Independent | Monthly visitor count |
| avg_discount_pct | Numerical | Independent | Average discount percentage applied |
| staff_count | Numerical | Independent | Number of staff in the store |
| inventory_availability_pct | Numerical | Independent | % of SKUs available in stock |
| competitor_distance_km | Numerical | Independent | Distance to nearest competitor (km) |
| holiday_flag | Binary | Independent | 1 if month contains a public holiday |
| customer_rating | Numerical | Independent | Average customer satisfaction rating (1–5) |
| **monthly_sales** | Numerical | **Dependent** | Total monthly revenue (£) — target variable |
| monthly_profit | Numerical | Excluded | Profit derived from sales; excluded to avoid data leakage |

### Variables Not Used in Regression
- `store_id`, `month`: Identifiers
- `monthly_profit`: Derived from sales — including it would cause data leakage
- `staff_count`, `holiday_flag`, `competitor_distance_km`: Excluded from final model to maintain parsimony; may be explored in future iterations

---

## Dependent and Independent Variables

- **Dependent Variable:** `monthly_sales`
- **Independent Variables Used:** `footfall`, `marketing_spend`, `inventory_availability_pct`, `avg_discount_pct`, `customer_rating`, regional dummies (`region_North`, `region_South`, `region_West`)

---

## Regression Approach

Three regression models were built and compared:

| Model | Type | Variables |
|-------|------|-----------|
| Model 1 | Simple Linear Regression | monthly_sales ~ footfall |
| Model 2 | Simple Linear Regression | monthly_sales ~ marketing_spend |
| Model 3 | Multiple Linear Regression | monthly_sales ~ footfall + marketing_spend + inventory_availability_pct + avg_discount_pct + customer_rating + region dummies |

All regression was performed using Ordinary Least Squares (OLS). Statistical significance was assessed using t-statistics and p-values.

---

## Dummy Variable Approach

Categorical variables were converted to binary (0/1) dummy variables:

### Region (Reference: East)
- `region_North` = 1 if North, 0 otherwise
- `region_South` = 1 if South, 0 otherwise
- `region_West` = 1 if West, 0 otherwise

### Store Type (Reference: Residential) — prepared but not in final model
- `type_High_Street`, `type_Airport`, `type_Mall`

The reference category (East; Residential) is left out to avoid the **dummy variable trap** (perfect multicollinearity). Coefficients on the included dummies show the difference in sales versus the reference group.

---

## Model Comparison Summary

| Metric | Model 1 (Footfall) | Model 2 (Marketing) | Model 3 (Multiple) |
|--------|-------------------|--------------------|--------------------|
| R-Squared | 0.7348 | 0.1574 | **0.8145** |
| Adjusted R² | 0.7331 | 0.1519 | **0.8089** |
| Significant Predictors | footfall | marketing_spend | footfall, marketing_spend, inventory, customer_rating, region_South, region_West |
| Business Usefulness | High | Moderate | **Very High** |

---

## Final Model Selected

**Model 3 — Multiple Linear Regression**

**Equation:**
```
monthly_sales = 114,227.28
              + 34.12 × footfall
              + 1.20 × marketing_spend
              + 2,541.14 × inventory_availability_pct
              − 45,050.80 × avg_discount_pct
              + 11,283.11 × customer_rating
              + 12,611.58 × region_North
              + 23,827.80 × region_South
              + 19,762.80 × region_West
```

Selected because it achieves the highest R² (0.8145), includes multiple actionable drivers, and uses dummy variables to control for regional variation.

---

## Business Recommendation

1. **Footfall is the primary driver** — invest in strategies to increase in-store visitor numbers (events, local marketing, click-and-collect)
2. **Maintain inventory availability above 90%** — each percentage point lost costs ~£2,541 in monthly sales
3. **Marketing generates positive returns** (~£1.20 per £1 spent) — but should be evaluated through its footfall-generating impact
4. **Improve customer ratings** — a 1-point improvement is associated with ~£11,283 more in monthly sales
5. **Do not over-rely on discounting** — avg_discount_pct is not statistically significant in this model
6. **South and West regions outperform East** — consider whether practices from those regions can be shared

---

## Assumptions and Limitations

- Regression identifies **association, not causation**
- Dataset covers only 4 months — seasonal effects may be incompletely captured
- 14 records excluded due to missing values
- `store_type` was not included in the final model — residual analysis shows potential bias for Airport/Mall vs High Street stores
- Omitted variables (store size, local demographics, competitor intensity) may bias some coefficients
- The model should be **validated with pilot testing** before major capital reallocation decisions

---

## Screenshots Included

| File | Content |
|------|---------|
| `simple_regression_output.png` | Simple regression output (footfall model) from regression_workbook.xlsx |
| `multiple_regression_output.png` | Multiple regression coefficient table from regression_workbook.xlsx |
| `residuals_preview.png` | Predicted vs. actual sales and residuals for all records |
| `model_comparison_preview.png` | Side-by-side model comparison table from regression_summary.xlsx |

---

## Files Reference

| File | Location | Description |
|------|----------|-------------|
| business_regression_data.xlsx | data/ | Original dataset |
| regression_workbook.xlsx | analysis/ | Full workbook with all regression outputs |
| model_comparison.md | analysis/ | Detailed model comparison write-up |
| residual_analysis.md | analysis/ | Residual analysis with business interpretation |
| regression_summary.xlsx | outputs/ | Clean comparison table of all models |
| final_recommendation.md | outputs/ | Business recommendation for leadership |
| model_equations.md | outputs/ | Regression equations and coefficient explanations |
| README.md | / | This file |
