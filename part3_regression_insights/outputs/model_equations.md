# Model Equations & Variable Explanations
### Regression Analysis — Monthly Sales Drivers | Retail Chain

---

## 1. Simple Regression Equations

Simple linear regression takes the form:

```
monthly_sales = Intercept + (Coefficient × Predictor Variable)
```

The intercept is the predicted monthly sales when the predictor variable equals zero. The coefficient shows how much monthly sales changes for each one-unit increase in the predictor.

---

### Model 1: Footfall → Monthly Sales

```
monthly_sales = 446,410.58 + 35.6780 × footfall
```

| Element | Value | Meaning |
|---|---|---|
| Intercept | 446,410.58 | Baseline predicted sales if footfall is zero (theoretical baseline) |
| Coefficient | 35.6780 | Each 1 additional visitor is associated with ₹35.68 more in monthly sales |
| R-squared | 0.7363 | Footfall alone explains 73.6% of variation in monthly sales |
| P-value | < 0.001 | Highly statistically significant |

**Business Language:** A store with 6,000 monthly visitors is predicted to generate approximately ₹660,000 in sales. A store with 9,000 visitors is predicted to generate approximately ₹767,000. Increasing footfall by 1,000 visitors is associated with approximately ₹35,700 more in monthly revenue.

---

### Model 2: Marketing Spend → Monthly Sales

```
monthly_sales = 560,777.35 + 2.1296 × marketing_spend
```

| Element | Value | Meaning |
|---|---|---|
| Intercept | 560,777.35 | Predicted sales with zero marketing investment |
| Coefficient | 2.1296 | Each ₹1 increase in marketing spend is associated with ₹2.13 more in sales |
| R-squared | 0.1672 | Marketing spend explains 16.7% of sales variation |
| P-value | < 0.001 | Highly significant |

**Business Language:** The implied return on marketing spend is approximately 2.1:1 in sales terms (note: this is a gross association, not a net ROI calculation — costs and margins are excluded). Marketing is a significant predictor, but its explanatory power is much lower than footfall, suggesting it is one contributing factor among many.

---

### Model 3: Inventory Availability → Monthly Sales

```
monthly_sales = 484,814.26 + 2,217.83 × inventory_availability_pct
```

| Element | Value | Meaning |
|---|---|---|
| Intercept | 484,814.26 | Predicted sales if inventory availability is 0% (theoretical) |
| Coefficient | 2,217.83 | Each 1 percentage point improvement in availability → ₹2,218 more in sales |
| R-squared | 0.0131 | Explains only 1.3% of variation when used alone |
| P-value | 0.041 | Statistically significant at 5% level |

**Business Language:** While inventory availability is statistically significant, it explains very little of monthly sales variation on its own. However, its significance in the multiple regression model (p < 0.001) confirms it is an important contributing factor in combination with other drivers.

---

### Model 4: Average Discount % → Monthly Sales

```
monthly_sales = 697,835.63 − 138,730.45 × avg_discount_pct
```

| Element | Value | Meaning |
|---|---|---|
| Intercept | 697,835.63 | Predicted sales at zero discount |
| Coefficient | −138,730.45 | Each 1-unit (100%) increase in discount rate → ₹138,730 lower sales |
| R-squared | 0.0083 | Explains less than 1% of variation |
| P-value | 0.104 | Not statistically significant at 5% level |

**Business Language:** The negative coefficient suggests that higher discounting is weakly associated with lower — not higher — monthly sales. This may indicate that discounting tends to occur in underperforming periods rather than driving additional volume. However, this relationship is not statistically reliable and should not be used for prediction.

---

## 2. Multiple Regression Equation

Multiple regression takes the form:

```
monthly_sales = Intercept + (β₁ × X₁) + (β₂ × X₂) + ... + (βₙ × Xₙ)
```

Each coefficient (β) represents the change in monthly sales associated with a one-unit increase in that predictor, **holding all other predictors constant**. This is the key advantage over simple regression — it isolates each variable's contribution.

---

### Model 6: Full Multiple Regression (Final Model)

```
monthly_sales = 89,736.69
             + 1.1546  × marketing_spend
             + 27.9912 × footfall
             − 49,804.12 × avg_discount_pct
             + 3,026.30 × staff_count
             + 3,032.60 × inventory_availability_pct
             + 16,166.63 × holiday_flag
             + 7,710.78  × region_North
             + 19,692.49 × region_South
             + 19,831.83 × region_West
             + 39,019.46 × store_Airport
             + 17,888.22 × store_High Street
             + 29,744.71 × store_Mall
```

**R-squared: 0.8337 | Adjusted R-squared: 0.8272 | F-statistic p-value: 7.74e-112**

---

## 3. Coefficient Explanations

### Numerical Variables

| Variable | Coefficient | Significance | Business Meaning |
|---|---|---|---|
| `const` (Intercept) | 89,736.69 | * (p=0.039) | Theoretical baseline when all predictors are zero. Not meaningful on its own — extrapolation beyond data range. |
| `marketing_spend` | +1.1546 | *** | Every ₹1 increase in monthly marketing spend is associated with ₹1.15 more in monthly sales, controlling for all other factors. |
| `footfall` | +27.9912 | *** | Every additional visitor to the store is associated with ₹27.99 more in monthly sales, controlling for all other factors. |
| `avg_discount_pct` | −49,804.12 | ns (p=0.172) | Every 1-unit (100%) increase in average discount rate is associated with ₹49,804 less in sales. **Not statistically significant — do not use for prediction.** |
| `staff_count` | +3,026.30 | * (p=0.016) | Every additional staff member is associated with ₹3,026 more in monthly sales. May reflect that busier/larger stores have both more staff and more sales (possible confounding). |
| `inventory_availability_pct` | +3,032.60 | *** | Every 1 percentage point improvement in product availability is associated with ₹3,033 more in monthly sales, controlling for other factors. Operationally actionable. |
| `holiday_flag` | +16,166.63 | * (p=0.014) | Months designated as holiday periods generate approximately ₹16,167 more in sales than equivalent non-holiday months. |

### Dummy Variable Coefficients (Categorical Variables)

Dummy variables represent group membership. Their coefficient shows the sales difference compared to the **reference category**.

---

## 4. Dummy Variable Explanation

Categorical variables (region, store_type) cannot be directly entered into a regression equation as text. They must be converted into **binary (0/1) indicator variables** — called dummy variables.

### How Dummy Variables Work

For a categorical variable with K categories, we create **K−1 dummy variables**. The omitted category becomes the **reference category**. All other category coefficients are interpreted as the difference from that reference.

**Example:**
```
region has 4 values: East, North, South, West
→ Create 3 dummies: region_North, region_South, region_West
→ Omit: region_East (reference)
→ Coefficient for region_South = how much more (or less) South sells vs East
```

### Reference Categories Used

| Variable | Reference Category | Reason for Selection |
|---|---|---|
| `region` | **East** | Alphabetically first; allows other regions to be compared against a meaningful baseline |
| `store_type` | **Residential** | The most common and operationally standard store format; a natural benchmark |

**Why we drop the reference category:** Including all K dummies would create perfect multicollinearity — one dummy is always predictable from the others. This is called the **dummy variable trap**. Dropping one category avoids this mathematical problem without losing any information.

### Region Dummy Coefficients

| Dummy Variable | Coefficient | Significance | Business Meaning |
|---|---|---|---|
| `region_North` | +7,710.78 | ns (p=0.269) | North region stores sell ~₹7,711 more than East (reference) per month. **Not statistically significant** — the East-North difference may be due to chance. |
| `region_South` | +19,692.49 | ** (p=0.006) | South region stores sell ~₹19,692 more than East (reference) per month, controlling for other factors. Statistically reliable. |
| `region_West` | +19,831.83 | ** (p=0.002) | West region stores sell ~₹19,832 more than East (reference) per month, controlling for other factors. Statistically reliable. |

### Store Type Dummy Coefficients

| Dummy Variable | Coefficient | Significance | Business Meaning |
|---|---|---|---|
| `store_Airport` | +39,019.46 | *** (p<0.001) | Airport stores generate ~₹39,019 more per month than Residential (reference) stores. Strongest store-type premium. |
| `store_High Street` | +17,888.22 | ** (p=0.003) | High Street stores generate ~₹17,888 more per month than Residential stores. |
| `store_Mall` | +29,744.71 | *** (p<0.001) | Mall stores generate ~₹29,745 more per month than Residential stores. |

**Note:** The Residential store type (reference category) does not appear in the equation. Its effect is captured by the intercept.

---

## 5. Final Model Selected

**Model 6 — Full Multiple Regression** is selected as the final model.

**Reason for Selection:**

The full multiple regression model is selected because it:

1. **Achieves the highest adjusted R-squared (0.8272)** — explaining 82.7% of monthly sales variation after penalising for model complexity. This is the appropriate metric for comparing models with different numbers of predictors.

2. **Is globally statistically significant** — the F-statistic of 128.3 (p = 7.74×10⁻¹¹²) confirms that the combination of predictors explains far more variance than would be expected by chance.

3. **Includes all major business drivers** — marketing investment, customer traffic, inventory health, staffing, store format, regional context, and seasonal effects are all represented.

4. **Outperforms simpler models on business interpretability** — the multiple regression isolates each driver's contribution while holding others constant, enabling more precise business recommendations than any single-variable model.

5. **Retains non-significant variables for transparency** — `avg_discount_pct` (p=0.172) and `region_North` (p=0.269) are retained in the full model to provide honest disclosure of which factors were tested and found weak, rather than selectively presenting only significant results.

**In business language:** The model tells leadership that sales are primarily driven by how many customers visit the store, whether inventory is available when they arrive, and how much is invested in marketing — with additional meaningful effects from store location type and geographic region. Discounting policy and regional differences between North and East are not statistically reliable predictors in this dataset.
