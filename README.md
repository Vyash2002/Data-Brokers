# CSE558 Midterm: Wine Reviews

**Is a wine score a property of the wine or of the critic?** This is statistical inference through hypothesis testing
on 120k WineEnthusiast reviews. It is the CSE558 Data Science midterm project (IIIT-Delhi, Monsoon 2026).

Team: *TODO: group name and members*

## Dataset

| | |
|---|---|
| Source | Kaggle, *Wine Reviews* by zynicide: https://www.kaggle.com/datasets/zynicide/wine-reviews |
| Downloaded | 22 October 2026 |
| File used | `data/winemag-data-130k-v2.csv` **only** (129,971 rows × 13 attributes, about 50 MB) |
| Not used | `winemag-data_first150k.csv` and `winemag-data-130k-v2.json` from the same download |

If `data/winemag-data-130k-v2.csv` is missing, download the Kaggle dataset and copy that one file into `data/`.

## Install and run

Python 3.10 or newer.

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace notebooks/midterm.ipynb   # or open it and Run All
```

A full run takes under a minute. It regenerates everything the report uses:

* `data/winemag_clean.csv`: the cleaned table (readable columns), which every test starts from
* `data/winemag_encoded.csv`: the encoded table behind the 3M size check
* `figures/*.pdf`: every plot
* `report/tables/*.tex`: every table, as LaTeX `tabular`s
* `report/numbers.tex`: every number quoted in the report, as `\val{key}` (`\input` it in the report preamble)
* `report/results.json`: the five test results in machine-readable form

No number in the report is typed by hand. A random seed (558) fixes the plot jitter.

## Layout

```
data/                    winemag-data-130k-v2.csv (raw, from Kaggle)
                         winemag_clean.csv, winemag_encoded.csv (written by the notebook)
notebooks/midterm.ipynb  all cleaning, encoding, EDA and tests
figures/                 plots (PDF, for LaTeX)
report/                  numbers.tex, results.json, tables/ (the LaTeX report goes here too)
```

Both prepared tables have 119,895 rows. Their first column, `raw_row`, is the row id in the Kaggle file.

* `winemag_clean.csv` has the cleaned attributes:
  * `country`, `province`, `region_1`, `taster_name`, `title`, `variety`, `description`
  * `points`
  * `price` (capped at the IQR fence) and `price_raw` (observed dollars, empty where imputed)
  * `price_imputed`, `vintage`, `desc_words`
* `winemag_encoded.csv` has 26 attributes:
  * 5 numeric: `points`, `price`, `price_imputed`, `vintage`, `desc_words`
  * `country_*`: 10 one-hot columns (top 9 and Other)
  * `variety_*`: 11 one-hot columns (top 10 and Other)

## Where each table and figure comes from

| Output | Notebook section | Report section |
|---|---|---|
| `tables/features.tex` (key features, type, meaning) | 1 Dataset | 1 |
| size check (`\val{rowsClean}` × `\val{colsEnc}` = `\val{cellsEnc}`) | 2.12 Encoding and the 3M size check | 1 |
| `data/winemag_clean.csv`, `data/winemag_encoded.csv` | 2.12 | 2 |
| `tables/prep.tex` (issue → what we did → why) | 2.12 | 2 |
| `tables/step_log.tex` (rows and columns after every step) | 2.1–2.11 | 2–3 |
| `figures/fig_price_outliers.pdf` (boxplot, quantile plot, after capping) | 2.10 Price outliers | 2 |
| `tables/before_after.tex` | 3 Before and after | 3 |
| `figures/fig_points.pdf` (score histogram) | 4 EDA | 4 |
| `figures/fig_price_points.pdf` (points by price band) | 4 EDA | 4 |
| `figures/fig_tasters.pdf` (critic means at $20–40) | 4 EDA | 4 |
| `figures/fig_words.pdf` (note length vs points) | 4 EDA | 4 |
| T1 Old vs New World: F-test, then Welch t; pooled t, z, within-critic check | 5 T1 | 5 |
| T2 note length vs points: t on Pearson r; Welch t; `tables/t2_tasters.tex` | 5 T2 | 5 |
| T3 price band × 90+: χ² independence; `tables/t3_bands.tex`, `figures/fig_band90.pdf` | 5 T3 | 5 |
| T4 Gregutt vs Sullivan 90+ share: two-proportion z; `figures/fig_wa_vintages.pdf` | 5 T4 | 5 |
| T5 Gregutt vs Sullivan spread: F-test; `figures/fig_qq.pdf` | 5 T5 | 5 |
| `tables/tests_summary.tex` (all five tests) | 5 Summary | 5 |
| limitation figures (truncation, winery clustering) | 6 Limitations | 6 |

## Methods

The tests use only methods from the course lectures (L2–L8): descriptive statistics, Pearson skewness and kurtosis,
boxplots, quantile and Q-Q plots, Pearson correlation, one- and two-sample z and t tests (pooled and Welch),
F-test for two variances, χ² test for a variance, z-test for two proportions, and χ² test for independence.
