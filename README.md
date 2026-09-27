# The Ethics of Self-Driving Cars: Who Dies?

An exploratory data analysis of the [Moral Machine](https://www.moralmachine.net/) dataset — MIT Media Lab's large-scale study on how people around the world want autonomous vehicles to resolve unavoidable-accident ("trolley problem") scenarios.

## Overview

When a self-driving car faces an unavoidable crash, who should it save? The Moral Machine experiment presented millions of people worldwide with variations of this dilemma — save the passengers or the pedestrians, the young or the old, more people or fewer, law-abiders or jaywalkers, and so on — and recorded their choices.

This notebook explores that response data to see which groups people tend to save, how preferences shift across scenario types, and whether moral preferences differ by country and cultural cluster.

## Data

Two CSV files are used (not included in this notebook file — see **Setup**):

| File | Description |
|---|---|
| `moral_machine_responses.csv` | ~600,000 individual outcome records. Each row is one side of a scenario shown to a respondent. |
| `country_preferences.csv` | Pre-aggregated save rates by `country`, `scenario_type`, and `character_group`, plus a `cultural_cluster` label per country. |

### Key columns in `moral_machine_responses.csv`

- `outcome_id`, `country`, `device` — identifiers and metadata
- `scenario_type` — the moral dimension being tested (`Utilitarian`, `Fitness`, `Species`, `Age`, `Gender`, `Social Status`, `Random`)
- `character_group` — which side of the dilemma this row represents (e.g. `Old`, `Young`, `Fat`, `Fit`, `More`, `Less`, `Pets`, `Hoomans`, `Male`, `Female`, `High`, `Low`)
- `saved` — 1 if this character group was the one spared, 0 otherwise
- Per-character indicator columns — `man`, `woman`, `pregnant`, `stroller`, `old_man`, `old_woman`, `boy`, `girl`, `homeless`, `large_woman`, `large_man`, `criminal`, `male_executive`, `female_executive`, `female_athlete`, etc.
- Scenario context — `dies_if_car_swerves`, `pedestrians_vs_pedestrians`, `is_passengers`, `crossing_legality`, `num_characters`, `diff_num_characters`, `scenario_order`

## What the notebook does

1. **Load and inspect** the response data (`pandas.read_csv`, `.shape`, `.dtypes`, `.head()`, `.isnull().sum()`).
2. **Profile the dataset** — 600,217 rows × 33 columns, responses from 211 countries, 7 scenario types, and both Desktop and Mobile devices. `country` has ~5,670 missing values and `device` ~78,000.
3. **Check the outcome balance** — of all rows, 277,628 (~46%) represent the character group that was saved vs. 322,589 (~54%) that were not.
4. **Aggregate save rates**:
   - By `scenario_type` (e.g. Age ≈ 46%, Fitness ≈ 43%, Social Status ≈ 50%)
   - By `character_group` (e.g. `Hoomans` ≈ 78%, `More` ≈ 74%, `Young` ≈ 72% saved most often; `Pets` ≈ 19%, `Less` ≈ 21%, `Old` ≈ 21% saved least often)
   - By `country`
   - By the combination of `scenario_type` and `character_group`, sorted to surface the strongest and weakest preferences
5. **Visualize** the results with `matplotlib`/`seaborn` bar charts of save rate by character group, including a version colored (`hue`) by scenario type.
6. **Bring in cultural clusters** — loads `country_preferences.csv`, extracts a `country → cultural_cluster` lookup table, and left-merges it onto the response data so every response is tagged with its country's cultural cluster (reconciling country codes that appear in one file but not the other).
7. **Compare clusters** — filters to the "Age" scenario where the "Young" were saved, then compares save rates across the three cultural clusters, and spot-checks specific countries (USA, Japan, Brazil) against their assigned cluster.

## Findings so far

- Preferences clearly favor **saving more lives, the young, and humans over pets/animals**, and disfavor saving the elderly, fewer people, or overweight characters.
- Save rates vary meaningfully **by scenario type**, with "Social Status" and "Random" scenarios closer to a 50/50 split than "Fitness" or "Age" scenarios.
- Save rates for the same dilemma (saving the young) differ **across cultural clusters**, suggesting geography/culture shapes moral preference, consistent with the original Moral Machine research.

## Tools used

- Python, `pandas` for data loading/wrangling
- `matplotlib` and `seaborn` for visualization

## Setup

1. Download the Moral Machine dataset files and place them alongside the notebook:
   - `moral_machine_responses.csv`
   - `country_preferences.csv`
2. Open `The_Ethics_of_Self_Driving_Cars_Who_Dies?.ipynb` in Jupyter or Google Colab (a Colab badge is included at the top of the notebook).
3. Run all cells top to bottom.

## Possible next steps

- Turn the ad hoc groupings into a single reusable summary function/table.
- Add statistical tests (e.g. chi-square, confidence intervals) behind the observed differences in save rates.
- Build out a fuller cross-tab of scenario type × cultural cluster.
- Handle/report on the missing `country` and `device` values explicitly rather than leaving them as NaN.
