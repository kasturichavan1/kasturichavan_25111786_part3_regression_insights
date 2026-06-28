# Residual Analysis

This file looks at where my final model (the multiple regression) got things right and where it got things wrong. A residual is just: **actual sales minus predicted sales**. A positive residual means the store sold more than the model expected. A negative residual means the store sold less than the model expected.

All predicted values and residuals were calculated with live formulas in `analysis/regression_workbook.xlsx`, sheet `6_Predictions_Residuals`. A screenshot of this is saved as `screenshots/residuals_preview.png`.

## Top 5 stores the model under-predicted (biggest positive residuals)

| Store | Month | Region | Store type | Actual sales | Predicted sales | Residual |
|---|---|---|---|---|---|---|
| STR-1030 | 2025-02 | West | Residential | 820,519 | 714,620 | +105,899 |
| STR-1073 | 2025-03 | East | Residential | 813,317 | 713,693 | +99,623 |
| STR-1032 | 2025-01 | South | High Street | 914,544 | 815,773 | +98,771 |
| STR-1050 | 2025-04 | North | Residential | 735,787 | 638,540 | +97,246 |
| STR-1028 | 2025-04 | East | Mall | 713,611 | 618,466 | +95,145 |

**What this might mean for the business:** these stores sold a lot more than the model expected given their marketing spend, footfall, discounting, and inventory levels. Four of the five top over-performers are Residential stores, even though the model already accounts for store type. That suggests something about these specific Residential locations — maybe a strong local manager, a nearby event, or a factor not in the dataset (like local competition or store layout) — is driving extra sales that the model can't see.

## Top 5 stores the model over-predicted (biggest negative residuals)

| Store | Month | Region | Store type | Actual sales | Predicted sales | Residual |
|---|---|---|---|---|---|---|
| STR-1017 | 2025-03 | West | High Street | 685,379 | 844,620 | -159,241 |
| STR-1023 | 2025-02 | South | Mall | 627,172 | 755,437 | -128,265 |
| STR-1012 | 2025-01 | West | Residential | 595,468 | 716,546 | -121,079 |
| STR-1007 | 2025-02 | West | Mall | 800,452 | 900,272 | -99,821 |
| STR-1009 | 2025-01 | North | Residential | 641,865 | 740,315 | -98,450 |

**What this might mean for the business:** these stores sold a lot less than expected. One pattern I noticed: three of these five stores (STR-1023, STR-1012, STR-1007) had marketing spend well above the average (₹138,000–₹172,000 vs. an overall average of about ₹56,000), but their sales did not scale up the way the model expects from marketing spend. This could mean diminishing returns on marketing spend at high levels, or it could mean the extra marketing money was spent in months/locations where it just wasn't effective. Either way, it's a flag worth investigating before assuming "more marketing spend = more sales" everywhere.

## Is the model under-predicting or over-predicting certain types of stores?

Looking at both lists together:

- **Residential stores show up on both sides** (3 of the top 5 over-performers, 2 of the top 5 under-performers). This means the model isn't biased for or against Residential stores in general — it's more likely that individual store-level factors (not captured in this dataset) are driving these specific results.
- **Mall stores lean toward the over-predicted side** (2 of the top 5 negative residuals are Mall, vs. 1 of the top 5 positive residuals). This is a mild signal, not a strong one, but it's worth keeping an eye on — if this pattern holds with more data, the model may be slightly overestimating typical Mall performance.
- **West region appears three times in the negative-residual list** (STR-1017, STR-1012, STR-1007), which is more than you'd expect by chance if residuals were spread evenly across all four regions. Region wasn't included in the final model, so this could be a sign that region matters more than this model currently captures.

## Bottom line

The residuals don't show one obvious group of stores the model completely fails on, but there are two soft patterns worth flagging to leadership: (1) very high marketing spend doesn't always translate into proportionally higher sales, and (2) the West region shows up disproportionately among the stores the model overestimates. Neither of these is a hard conclusion — with only 4 months of data, it's possible these are one-off effects rather than a real pattern.
