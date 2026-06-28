# Part 3 — Regression-Based Business Insights

## Business problem

I'm looking at this from the point of view of a business analyst at a retail chain. Leadership wants to know what's actually driving monthly sales across their stores, so they can decide where to put money and effort — things like marketing spend, keeping shelves stocked, discounting, staffing, or which store types/regions to prioritize.

## Dataset description

The dataset (`data/business_regression_data.xlsx`) has 320 rows — that's 80 stores tracked over 4 months (January to April 2025). There's also a `data_dictionary` sheet in the same file that explains each column.

Each row is one store, one month, with these columns:

- `store_id`, `month` — which store and which month
- `region` — East, North, South, or West
- `store_type` — Mall, High Street, Residential, or Airport
- `marketing_spend` — how much was spent on marketing that month
- `footfall` — number of visitors that month
- `avg_discount_pct` — average discount given that month
- `staff_count` — number of staff
- `inventory_availability_pct` — how often products were actually in stock
- `competitor_distance_km` — distance to the nearest competitor (had 6 missing values)
- `holiday_flag` — whether the month had a major holiday (1 or 0)
- `customer_rating` — average customer rating (had 8 missing values)
- `monthly_sales` — what we're trying to explain
- `monthly_profit` — a second business metric, not used in this analysis

## Dependent and independent variables

**Dependent variable:** `monthly_sales` — this is what I'm trying to explain/predict.

**Independent variables I actually used in the models:**
- `marketing_spend`, `footfall`, `avg_discount_pct`, `inventory_availability_pct` (all numeric)
- `store_type` (turned into dummy variables — see below)

**Variables I looked at but didn't use as regression inputs, and why:**
- `customer_rating` — I did test it in a simple regression (it showed basically no relationship with sales), but I left it out of the final multiple regression since it wasn't adding anything useful.
- `staff_count`, `competitor_distance_km`, `holiday_flag` — these are in the dataset and could be interesting to explore in a future analysis, but the assignment only asked for at least 3 numeric variables plus 1 dummy variable, so I focused on the variables most directly tied to the business actions leadership is considering (marketing, inventory, discounting, store type).
- `monthly_profit` — this is a separate outcome metric, not something that should be used to *predict* sales, so I left it out entirely.
- `store_id`, `month` — these are identifiers, not real predictors.

**Variables that needed cleaning:**
- `competitor_distance_km` and `customer_rating` both had a small number of missing values (6 and 8 out of 320). I filled these in with the column median, which is a simple and common way to handle a small number of gaps without throwing away the whole row.

## Regression approach

I built two kinds of models:

1. **Simple regressions** — one predictor at a time against `monthly_sales`. I tried 5 of these: marketing_spend, footfall, avg_discount_pct, inventory_availability_pct, and customer_rating.
2. **One multiple regression** — combining marketing_spend, footfall, avg_discount_pct, inventory_availability_pct, and the store_type dummy variables all together.

All the regression math (slope, intercept, R-squared, p-values) was done with live Excel formulas (SLOPE, INTERCEPT, RSQ, LINEST, TDIST) inside `analysis/regression_workbook.xlsx`, not pasted-in numbers — so if you change a value in the original data sheet, the whole workbook recalculates.

## Dummy variable approach

`store_type` has four categories: Airport, High Street, Mall, Residential. I picked **High Street as the reference category** (it's the most common type in the data), so I only made three dummy columns: `st_Airport`, `st_Mall`, `st_Residential`. Every coefficient on these dummies is read as "compared to a High Street store." I didn't make a dummy for High Street itself, because that would make the dummy columns redundant with each other (this is explained more in `outputs/model_equations.md`).

## Model comparison summary

| Model | R-squared | Reliable? |
|---|---|---|
| marketing_spend only | 16.7% | Yes, but weak on its own |
| footfall only | 73.6% | Yes — strongest single variable |
| avg_discount_pct only | 0.8% | No |
| inventory_availability_pct only | 1.3% | Yes, but barely |
| customer_rating only | 0.1% | No |
| **Multiple regression (all combined)** | **82.1%** | **Yes — best overall model** |

Full comparison with business notes is in `analysis/model_comparison.md`.

## Final model selected

The **multiple regression model**, because it explains the most variation in sales (82.1%) and most of its variables are statistically significant. The full equation and what each coefficient means is in `outputs/model_equations.md`.

## Business recommendation (short version)

Footfall and marketing spend are the two strongest, most reliable drivers of monthly sales. Inventory availability and store type also matter, just less. Discounting and customer rating did not show a reliable link to sales in this data, so I wouldn't make decisions based on those two yet. The full recommendation, including risks and limitations, is in `outputs/final_recommendation.md`.

## Assumptions and limitations

- Missing values in `competitor_distance_km` and `customer_rating` were filled with the column median.
- The dataset only covers 4 months, so seasonal patterns (like festival season) aren't captured.
- Regression shows that variables move together, not that one *causes* the other. See the causation note in `outputs/final_recommendation.md`.
- A few variables in the multiple regression (avg_discount_pct, st_Mall) weren't statistically significant, so I'm not fully confident in those specific coefficients.

## Screenshots included

- `screenshots/simple_regression_output.png` — output of all 5 simple regression models
- `screenshots/multiple_regression_output.png` — output of the multiple regression model
- `screenshots/residuals_preview.png` — top 5 biggest positive and negative residuals
- `screenshots/model_comparison_preview.png` — the model comparison table

## How the repo is organized

```
data/                    → the raw dataset
analysis/                → the regression workbook + model comparison + residual analysis write-ups
outputs/                 → regression summary, model equations, and final recommendation
screenshots/             → evidence screenshots for each required output
```
