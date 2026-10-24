# CSE558 Midterm: Wine Reviews

**Is a wine's score about the wine, or about the critic who tasted it?** Hypothesis testing on 120k WineEnthusiast
reviews, for the CSE558 Data Science midterm (IIIT-Delhi, Monsoon 2026).

Team: *TODO: group name and members*

## Dataset

| | |
|---|---|
| Source | Kaggle, *Wine Reviews* by zynicide: https://www.kaggle.com/datasets/zynicide/wine-reviews |
| Downloaded | 22 October 2026 |
| File used | `data/winemag-data-130k-v2.csv` **only** (129,971 rows × 13 columns, about 50 MB) |
| Not used | `winemag-data_first150k.csv` and `winemag-data-130k-v2.json` from the same download |

If `data/winemag-data-130k-v2.csv` is missing, download the Kaggle dataset and copy that one file into `data/`.

## Install and run

Python 3.10 or newer.

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace notebooks/midterm.ipynb   # or open it and Run All
```

A full run takes under a minute. It writes:

* `data/winemag_clean.csv`: the cleaned data, which every test starts from
* `data/winemag_encoded.csv`: the encoded data behind the 3M size check
* `figures/*.pdf`: every plot

Every number in the report is copied from the notebook's printed output.

## Layout

```
data/                    winemag-data-130k-v2.csv (raw, from Kaggle)
                         winemag_clean.csv, winemag_encoded.csv (written by the notebook)
notebooks/midterm.ipynb  cleaning, encoding, exploring and the five tests
figures/                 plots (PDF, for LaTeX)
report/                  the LaTeX report (to do)
```

Both prepared tables have 119,895 rows. Their first column, `raw_row`, is the row number in the Kaggle file.

* `winemag_clean.csv` has the cleaned columns:
  * `country`, `province`, `region_1`, `taster_name`, `title`, `variety`, `description`
  * `points`
  * `price` (capped at the IQR fence) and `price_raw` (observed dollars, empty where the price was filled in)
  * `price_imputed`, `vintage`, `desc_words`
* `winemag_encoded.csv` has 26 columns:
  * 5 numeric: `points`, `price`, `price_imputed`, `vintage`, `desc_words`
  * `country_*`: 10 one-hot columns (top 9 and Other)
  * `variety_*`: 11 one-hot columns (top 10 and Other)

## Where each output comes from

| Output | Notebook section |
|---|---|
| size check (119,895 × 26 = 3,117,270) and the two data files | 2.12 Encode categories and check the size |
| `figures/fig_price_outliers.pdf` (boxplot, quantile plot, after capping) | 2.10 Handle very high prices |
| before vs after table | 3 Before vs after cleaning |
| `figures/fig_points.pdf`, `fig_price_points.pdf`, `fig_tasters.pdf`, `fig_words.pdf` | 4 Exploring the data |
| T1 Old vs New World: F-test, then Welch t; pooled t as the other test | 5 T1 |
| T2 review length vs score: t on Pearson r; Welch t as the other test | 5 T2 |
| T3 price band × 90+: χ² independence; `figures/fig_band90.pdf` | 5 T3 |
| T4 Gregutt vs Sullivan, 90+ share: two-proportion z; `figures/fig_wa_vintages.pdf` | 5 T4 |
| T5 Gregutt vs Sullivan, spread: F-test; `figures/fig_qq.pdf` | 5 T5 |
| summary of the five tests | 5 Summary |

## Methods

The tests use only methods from the course lectures (L2–L8): descriptive statistics, Pearson skewness and kurtosis,
boxplots, quantile and Q-Q plots, Pearson correlation, two-sample t tests (pooled and Welch), F-test for two
variances, χ² test for a variance, z-test for two proportions, and χ² test for independence.
