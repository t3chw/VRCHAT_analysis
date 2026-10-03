# VRCHAT_analysis

A retention analysis of VRChat, built from every public Steam review of the game (app 438100, downloaded 28 Aug 2026). A review shows when someone started playing and when they last played, so it can be used to estimate how long players stay.

The reviews hold no text and no review or user IDs.

## Notebooks

Run them in order. Part 1 saves the retention table that Parts 2 and 3 read.

1. `VRChat_1_who_stays.ipynb`: loads the data and measures who stays (survival curves, playtime bands, the two-week rule).
2. `VRChat_2_who_leaves_sooner.ipynb`: looks at who leaves sooner and when the risk of leaving is highest (Cox models, validation). The Cox fits take a few minutes.
3. `VRChat_3_designing_a_test.ipynb`: designs a fair test of a change to player retention (who to test, when, and how many players).

## Data

- `data/vrchat_steam_reviews.csv.gz`: the raw review data.
- `data/retention_table.csv`: created by notebook 1 and not tracked in git.

## Setup

```bash
pip install pandas numpy matplotlib lifelines jupyter
jupyter notebook
```
