# Model Equations

This file explains the regression equations I built, what each coefficient means, and how I handled the dummy variables. All the numbers come from `analysis/regression_workbook.xlsx`.

## Dummy variable approach

`store_type` has four categories: Airport, High Street, Mall, and Residential. You can't put a text category straight into a regression, so I converted it into 1/0 (dummy) columns.

I used **High Street as the reference category** because it's the most common store type in the dataset (116 of 320 records). This means I only created three dummy columns — `st_Airport`, `st_Mall`, `st_Residential` — instead of four. If I had created a dummy for High Street too, the four dummy columns would always add up to 1, which causes a problem called multicollinearity (the model can't tell the variables apart, because one is always exactly predictable from the others). Leaving out High Street avoids this. Every coefficient on a store-type dummy is read as "compared to a High Street store."

## Simple regression equations

These each use one predictor at a time, with `monthly_sales` as the outcome:

1. **monthly_sales = 560,777.35 + 2.13 × marketing_spend**
   For every extra ₹1 spent on marketing, sales go up by about ₹2.13, on average — but this model alone only explains 16.7% of why sales differ between stores.

2. **monthly_sales = 446,410.58 + 35.68 × footfall**
   For every extra visitor in a month, sales go up by about ₹35.68, on average. This is the strongest single-variable model — it explains 73.6% of the differences in sales.

3. **monthly_sales = 697,835.63 − 138,730.45 × avg_discount_pct**
   This suggests more discounting is linked to lower sales, but the relationship isn't statistically reliable (p = 0.10), so I wouldn't trust this number on its own.

4. **monthly_sales = 484,814.26 + 2,217.83 × inventory_availability_pct**
   Better stock availability is linked to slightly higher sales, and this one is statistically significant, but it only explains 1.3% of the overall picture by itself.

5. **monthly_sales = 701,366.86 − 5,284.87 × customer_rating**
   No real relationship was found here (p = 0.64) — I wouldn't read anything into this coefficient.

## Multiple regression equation (final model)

**monthly_sales =**
**136,913.65**
**+ 1.17 × marketing_spend**
**+ 33.66 × footfall**
**− 58,920.81 × avg_discount_pct**
**+ 3,001.20 × inventory_availability_pct**
**+ 21,905.71 × st_Airport**
**+ 11,433.46 × st_Mall**
**− 16,575.85 × st_Residential**

This model explains 82.1% of the differences in monthly sales across stores (R-squared = 0.8214).

### What each part means in plain terms

- **Intercept (136,913.65):** the expected sales for a High Street store if marketing spend, footfall, discounting, and inventory were all zero. This number on its own isn't meaningful for the business — it's just the starting point the formula is built from.
- **marketing_spend (+1.17, p < 0.0001, significant):** holding everything else constant, every extra ₹1 of marketing spend is linked to about ₹1.17 more in monthly sales.
- **footfall (+33.66, p < 0.0001, significant):** holding everything else constant, every extra visitor is linked to about ₹33.66 more in monthly sales. This is the single biggest driver in the model.
- **avg_discount_pct (−58,920.81, p = 0.11, NOT significant):** the negative sign suggests heavier discounting might be linked to lower sales, but the p-value is above 0.05, so this result isn't reliable enough to act on.
- **inventory_availability_pct (+3,001.20, p < 0.0001, significant):** every 1 percentage point improvement in stock availability is linked to about ₹3,001 more in monthly sales.
- **st_Airport (+21,905.71, p = 0.02, significant):** Airport stores sell about ₹21,906 more per month than a comparable High Street store, after accounting for marketing, footfall, discounting, and inventory.
- **st_Mall (+11,433.46, p = 0.08, borderline/not quite significant):** Mall stores appear to sell more than High Street stores, but the p-value is just above the usual 0.05 cutoff, so I'm treating this one with caution.
- **st_Residential (−16,575.85, p = 0.007, significant):** Residential stores sell about ₹16,576 less per month than a comparable High Street store, holding the other factors constant.

## Final model selected

I'm using the **multiple regression model** as my final model.

## Why I picked it

It explains far more of the variation in sales (82.1%) than any single-variable model, and most of its coefficients are statistically significant. It also matches what leadership actually cares about — marketing spend, inventory, and store type are all business levers they're considering. A simple regression with just one variable would have left out everything leadership wanted to compare.
