# Motor Vehicle Fatality Risk Across the United States

A Python port of a 2023 state-level investigative analysis. The original work was done in Excel and is preserved here in `docs/original_report.pdf`; this repository recreates the analysis in pandas, matplotlib, statsmodels, and scipy so the methodology is verifiable in code and reproducible from raw data files.

**Read the rendered work directly:**

- [Read the analysis](analysis.html): full notebook with code, plots, and findings, no Python install needed.
- [Open the original 2023 report (PDF)](docs/original_report.pdf): the polished narrative version of the analysis.

## Question

What state-level factors are associated with motor vehicle fatality risk across the United States, and how strong are those associations?

## Data

Four data sources, all state-level (50 states, one row each), joined into a single modelling panel.

| Variable | Year | Source |
|---|---|---|
| `fatalities_per_100k_1994_2015` (response) | 1994 to 2015 | NHTSA FARS, normalised per 100,000 residents |
| `avg_miles_per_driver_2022` | 2022 | The Zebra, derived from Federal Highway Administration |
| `police_count_2020` | 2020 | Statista |
| `alcohol_gallons_per_capita_2023` | 2023 | World Population Review |
| `pct_pickup_trucks_2022` | 2022 | Industry data |

The response variable is the long-run state-level fatality rate per 100,000 residents averaged over the 22-year window 1994 to 2015. Per-100,000 rather than per-million-vehicle-miles or per-licensed-driver was chosen because it expresses risk in a unit (population) that every resident shares directly, and it produces non-decimal numbers that are easy to compare at a glance.

## Method

Simple linear regression and Pearson correlation for each predictor against the response, followed by a Z-score composite of the two strongest predictors and a multivariate OLS model with all four predictors. Statsmodels for inference, matplotlib for plots, scipy.stats for correlations.

## Headline findings

The strongest single predictor is average miles per driver (r = 0.66). Pickup truck share is a moderate secondary signal (r = 0.45) that overlaps with the rural-state pattern. Raw police count is uninformative without per-capita normalisation. Alcohol consumption per capita correlates weakly negatively, likely picking up urbanisation and tourism inflows rather than risky driving behaviour. The Z-score composite of miles-per-driver and pickup-truck share is the strongest predictor overall (r = 0.70, R² = 0.50). See `analysis.html` for the full discussion.

## Files

| File | Purpose |
|---|---|
| `analysis.ipynb` | Jupyter notebook with code, plots, and discussion. |
| `analysis.html` | Rendered HTML of the notebook, viewable without Python. |
| `data/state_panel.csv` | 50-state cross-section: four predictors plus the response. The modelling input. |
| `data/nhtsa_raw_1994_2015.csv` | Raw NHTSA state-year panel (1,100 rows) used to derive the response variable. |
| `data/us_fatality_rate_by_year.csv` | US-wide fatality rate trend 1994 to 2015. |
| `docs/original_report.pdf` | The 2023 Excel-based original report. |
| `requirements.txt` | Python dependencies. |
| `LICENSE` | MIT. |

## How to reproduce

```bash
pip install -r requirements.txt
jupyter notebook analysis.ipynb
```

Or render to HTML in one step:

```bash
jupyter nbconvert --to html --execute analysis.ipynb
```

## License

MIT. See `LICENSE`.
