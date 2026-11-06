# Excess comovement across US industries, 2005–2026

Replication code and data for the empirical exercise in the AI Chronicles article
**"Efficient Firms, Fragile System: Can We Measure AI Herding Before It Bites?"**
by Giorgia Galeazzi, PhD — Research Economist | Developer.

[Link to the article: to add]

## What this measures

The question: once standard risk factors are removed, do US industries move together
more than they used to?

For each rolling window, the script:

1. regresses each of the 49 industries' daily excess returns on a set of risk factors;
2. keeps the residuals;
3. averages the pairwise correlations of those residuals across all industries.

It does this for three measures:

| Measure | Factors removed |
|---|---|
| Total (raw) comovement | none |
| Residual after 3 factors | market, size, value |
| Residual after 6 factors | market, size, value, profitability, investment, momentum |

The calculation runs twice, with 126-day (~6 months) and 63-day (~3 months) windows, as a
robustness check.

**What this does not measure.** A rise in residual comovement is *consistent with* growing
similarity in how markets process information. It is not evidence that AI causes it. Common
macro shocks, passive investing, retail trading and investment themes (including the AI trade
itself) can produce the same pattern. The exercise is illustrative, not causal.

## Data

All data come from the [Kenneth French Data Library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html)
(free, daily). The script downloads them directly:

| File | Content |
|---|---|
| `49_Industry_Portfolios_daily_CSV.zip` | 49 industry portfolios, value-weighted returns |
| `F-F_Research_Data_5_Factors_2x3_daily_CSV.zip` | Fama-French 5 factors and the risk-free rate |
| `F-F_Momentum_Factor_daily_CSV.zip` | Momentum factor |

Sample: January 2005 to the latest date available in the library (last observation in the
published results: 24 September 2026). Missing values coded as −99.99 are set to `NA`.
Returns are converted from per cent to decimals.

## Requirements

- R (4.1 or later recommended)
- Packages: `data.table`, `ggplot2`
- An internet connection (the data are downloaded at run time)

```r
install.packages(c("data.table", "ggplot2"))
```

## How to run

Set the working directory to the folder where you want the outputs, then run the script:

```r
setwd("path/to/output/folder")
source("excess_comovement.R")
```

A full run takes a few minutes, depending on your machine.

## Settings

At the top of the script:

| Setting | Default | Meaning |
|---|---|---|
| `start_date` | `2005-01-01` | First date in the sample |
| `windows` | `c(126, 63)` | Rolling window lengths, in trading days |
| `step` | `5` | Compute every 5 trading days (weekly); set to 1 for daily |
| `min_frac` | `0.8` | Minimum share of non-missing days an industry needs to enter a window |

## Outputs

| File | Content |
|---|---|
| `excess_comovement_v3.csv` | Full weekly series for both windows: `window`, `date`, `raw`, `ff3`, `ff6`, `n_ind` |
| `excess_comovement_v3_summary.csv` | Period averages (2005–09, 2010–14, 2015–19, 2020–22, 2023–26) by window |
| `excess_comovement_v3.png` | Article chart: residual comovement after 3 and 6 factors, 126-day window |
| `robustness_windows_v3.png` | Robustness chart: 6-factor residual comovement, 126- vs 63-day windows |

The console also prints a summary table comparing the two windows and the date of the final
observation.

`excess_comovement_results.xlsx` (in this repository) contains the same period averages
computed with live formulas from the full weekly series, for readers who prefer a spreadsheet.

## Main results

Average residual correlation after 6 factors:

| Period | 126-day window | 63-day window |
|---|---|---|
| 2005–09 | 0.006 | 0.005 |
| 2010–14 | 0.005 | 0.005 |
| 2015–19 | 0.008 | 0.008 |
| 2020–22 | 0.033 | 0.034 |
| 2023–26 | 0.040 | 0.039 |

In summary:

- **Total comovement** has fallen to its lowest level since the data begin in 2005.
- **A gradual rise between 2015 and 2019** in residual comovement after 3 factors disappears
  once profitability, investment and momentum are added.
- **A jump in 2020** survives both factor models. Unexplained comovement rose to more than four
  times its 2015–19 level, has stayed there since, and has edged up a little further since 2023.
- **The broad pattern is similar** with 63-day windows, so the result is not particularly
  sensitive to window length. This does not rule out that the pandemic itself changed how
  industries move together.

## Caveats

- **Overlapping windows.** Neighbouring observations share most of their data, so the series is
  serially dependent. Do not treat weekly points as independent observations.
- **Omitted factors.** Residual comovement is defined relative to a factor model. A common driver
  missing from the model, such as an investment theme, shows up as residual comovement.
- **Industry level only.** Industry portfolios average away firm-level behaviour. The exercise
  says nothing about which institutions trade, or which models they use.
- **No attribution to AI.** Nothing in these data identifies AI use. Linking comovement to shared
  AI infrastructure would require data on provider dependencies, which are not currently
  available in published supervisory statistics.

## Citation

If you use this code, please cite:

> Galeazzi, G. (2026). *Excess comovement across US industries, 2005–2026* [Code and data].
> Replication material for "Efficient Firms, Fragile System", AI Chronicles.
> https://github.com/giorgiagaleazzi/AI_Chronicles/blob/main/Efficient%20Firms%2C%20Fragile%20System

Data: Kenneth R. French, Data Library, Tuck School of Business at Dartmouth.

