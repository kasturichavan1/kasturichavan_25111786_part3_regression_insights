# Final Recommendation

## Which factors appear most strongly associated with monthly sales?

Based on the regression analysis, two factors stand out clearly above the rest:

1. **Footfall** — by far the strongest single factor. On its own, footfall explains about 74% of the differences in monthly sales between stores.
2. **Marketing spend** — a smaller but still real and statistically reliable factor. More marketing spend is linked to higher sales, even after accounting for footfall, discounting, inventory, and store type.

Two more factors showed a real, statistically reliable link to sales, though smaller in size:

3. **Inventory availability** — better stock availability is linked to higher sales.
4. **Store type** — Airport stores tend to sell more than High Street stores, and Residential stores tend to sell less, even after controlling for the other variables.

## Which variables should leadership focus on?

I'd suggest leadership prioritize **footfall and marketing spend** first, since these have the clearest and strongest evidence behind them. **Inventory availability** is also worth attention — it's a smaller effect, but it's something the business can directly control through operations, which makes it a practical lever even if the dollar impact per store is modest.

## Which variables should not be over-interpreted?

- **avg_discount_pct** — the simple and multiple regression both showed this variable was *not* statistically significant (p-values of 0.10 and 0.11). I would not recommend changing discounting strategy based on this analysis alone.
- **customer_rating** — showed essentially no relationship with monthly sales in this dataset (p = 0.64). That doesn't mean customer rating is unimportant to the business overall — it likely matters for things like repeat visits or brand reputation — it just didn't show up as a driver of *this month's* sales number specifically.
- **st_Mall** — the coefficient suggests Mall stores might sell a bit more than High Street stores, but the p-value (0.08) is just above the usual 0.05 cutoff, so I'd treat this one as a "maybe" rather than a confirmed finding.

## What business action would I recommend?

Given the evidence, I would recommend:

- **Prioritize footfall-driving initiatives** (location selection, local promotions, store visibility) since footfall has the single largest measured link to sales.
- **Continue investing in marketing spend**, since it shows a real, positive, and statistically reliable link to sales — but see the risk note below about diminishing returns.
- **Keep inventory availability high**, since stockouts are linked to lower sales, even if the effect size is smaller than footfall or marketing.
- **Investigate Residential store performance specifically** — these stores show consistently lower sales than other store types even after controlling for marketing, footfall, and inventory, which suggests something structural about how Residential stores operate that this dataset doesn't capture.
- **Hold off on changing discounting strategy based on this analysis** — the data doesn't give a statistically reliable signal either way.

## What risks or limitations should leadership keep in mind?

- **Only 4 months of data** (January–April 2025) for 80 stores. Seasonal effects (e.g. festival months, summer slowdowns) are not captured, and the model might behave differently across a full year.
- **The residual analysis (see `analysis/residual_analysis.md`) showed that very high marketing spend doesn't always translate into proportionally higher sales** — three of the five biggest "sales came in lower than expected" cases had marketing spend far above average. This hints at possible diminishing returns at high spend levels, which this model doesn't explicitly capture.
- **Region was not included in the final model**, but the residual analysis showed a mild pattern of the model overestimating sales in the West region. This is worth a closer look with more data before drawing firm conclusions.
- **Missing data was filled in for two columns** (`competitor_distance_km` and `customer_rating`) using the column median, which is a reasonable approach for a small number of gaps but could slightly understate the true relationship for those variables.

## Why does regression show association but not automatically prove causation?

Regression measures how strongly two variables move together in the data we already have — it doesn't tell us *why* they move together. For example, footfall and sales are strongly linked, but that doesn't necessarily mean increasing footfall artificially (say, through a one-off event) would increase sales by the same amount the model predicts. It's also possible that some other factor — like store location quality — drives both footfall and sales at the same time, making them look connected without footfall being the actual cause of higher sales. To really test causation, the business would need a controlled experiment (e.g. increasing footfall or marketing spend in some stores but not others and comparing results), rather than just observing existing patterns in historical data.
