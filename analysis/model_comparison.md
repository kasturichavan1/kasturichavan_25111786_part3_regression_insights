# Model Comparison

I tested several regression models to see which one best explains monthly sales. All the numbers below come straight from `analysis/regression_workbook.xlsx` (sheets 4 to 7), so anyone can open the workbook and check them.

## The models I tried

| Model | Variables used | R-squared | Significant variables (p < 0.05) | Useful for the business? | What it misses |
|---|---|---|---|---|---|
| Simple: sales ~ marketing_spend | marketing_spend | 16.7% | Yes | A bit useful on its own, but only explains a small slice of why sales change | Doesn't account for footfall, store type, or anything else |
| Simple: sales ~ footfall | footfall | 73.6% | Yes | Very useful — footfall alone explains almost three-quarters of the differences in sales | Still doesn't tell us *why* footfall is high or low, or what to actually do about marketing/discounts |
| Simple: sales ~ avg_discount_pct | avg_discount_pct | 0.8% | No | Not useful by itself, the relationship is too weak to trust | Discounting might still matter in combination with other factors, just not on its own |
| Simple: sales ~ inventory_availability_pct | inventory_availability_pct | 1.3% | Yes (borderline) | Statistically significant but explains very little of the overall picture | Too small an effect on its own to base a decision on |
| Simple: sales ~ customer_rating | customer_rating | 0.1% | No | Not useful — almost no relationship found | Customer rating might matter for things other than monthly sales (e.g. loyalty, long-term retention) |
| **Multiple regression (final model)** | marketing_spend, footfall, avg_discount_pct, inventory_availability_pct, store_type (dummy variables) | **82.1%** | marketing_spend, footfall, inventory_availability_pct, and store type (Airport, Residential) | **Yes — this is the most useful model.** It explains the largest share of sales differences and shows several variables working together | Doesn't include region, doesn't prove causation, and a few variables (avg_discount_pct, st_Mall) are not statistically reliable in this version |

## What stood out to me

- **Footfall by itself already explains most of the story.** A simple regression using just footfall got an R-squared of 73.6%, which is surprisingly high for a single variable.
- **Adding more variables helped, but not by a huge amount.** Going from the footfall-only model (73.6%) to the full multiple regression (82.1%) added about 8.5 percentage points of explanatory power. Marketing spend, inventory availability, and store type each added a bit more on top of footfall.
- **Discounting and customer rating did not show a reliable relationship with sales**, at least not in this dataset. I was expecting discounting to matter more, but the p-value was too high (0.10) for me to trust it.
- **Store type matters once you control for the other factors.** Airport stores and Residential stores both showed real differences in sales compared to High Street stores (the reference category), even after accounting for marketing spend, footfall, discounting, and inventory.

## Why I picked the multiple regression as my final model

It has the highest R-squared (82.1%), it includes the variables leadership actually asked about (marketing, inventory, store type), and most of its variables are statistically significant. The simple regressions are good for understanding one relationship at a time, but they leave out everything else that's going on. The multiple regression gives a much more complete and balanced view of what's driving sales.

## Limitations of the comparison

- A higher R-squared doesn't automatically mean a model is "correct" — it just means it explains more of the variation in this particular dataset.
- I only had 4 months of data for 80 stores, so the model might not generalize well to other seasons or years.
- `avg_discount_pct` was not significant in either the simple or multiple model, so I'm not confident in its coefficient sign or size.
- `region` was not included as a variable in the final model (see `model_equations.md` for why), so any regional patterns are not captured here.
