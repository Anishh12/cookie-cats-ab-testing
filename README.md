# Cookie Cats A/B Test: Where Should the First Gate Go?

## Problem
Cookie Cats is a mobile puzzle game. A "gate" stops players until they wait or pay.
The first gate was at **level 30** (`gate_30`, control). For half of new players it was moved to **level 40** (`gate_40`, test).

**Question:** Does moving the gate to level 40 change player retention or engagement?

## Data
[Mobile Games A/B Testing (Kaggle)](https://www.kaggle.com/datasets/yufengsui/mobile-games-ab-testing) , 90,189 players.

| Column | Meaning |
|---|---|
| `userid` | Unique player id |
| `version` | `gate_30` or `gate_40` |
| `sum_gamerounds` | Rounds played after install |
| `retention_1` | Came back after 1 day? |
| `retention_7` | Came back after 7 days? |

## Method
1. **Data checks:** no missing values, no duplicate users, group sizes about equal (44,700 vs 45,489).
2. **Outlier:** removed one impossible player (49,854 rounds; next highest is 2,961).
3. **Retention (True/False):** two-proportion z-test.
4. **Game rounds (skewed):** Mann-Whitney U test.
5. **Bootstrap:** 2,000 re-draws to confirm the 7-day retention result.
6. **Power analysis:** checked whether the test had enough players.
7. **Activity buckets:** compared retention rates by how much players played (descriptive only).

## Results

| Metric | gate_30 | gate_40 | Significant? |
|---|---|---|---|
| 1-day retention | 44.8% | 44.2% | No (p = 0.074) |
| 7-day retention | 19.0% | 18.2% | **Yes** (p = 0.0016) |
| Game rounds (median) | 17 | 16 | No, borderline (p = 0.051) |

- **Bootstrap:** the 7-day retention gap is +0.82 percentage points (95% interval +0.33 to +1.28), with `gate_30` ahead in essentially all re-draws.
- **Power:** about 35,500 players per group were needed for 80% power; each group had about 45,000 (power about 88%).

## Recommendation
**Keep the gate at level 30.** Moving it to level 40 does not help and slightly lowers 7-day retention.

## Limitations
- The effect is small (under 1 percentage point).
- Only day-1 and day-7 retention are available, with no revenue data, so the money impact is unknown.
- The data cannot show why players leave.
- Group sizes are slightly uneven (split check p about 0.009), a minor randomization warning.

## Files
- `cookie_cats_ab_test_simple.ipynb` , full analysis, step by step
- `cookie_cats.csv` , data (download from Kaggle)

## Tools
Python, pandas, NumPy, SciPy, statsmodels, matplotlib
