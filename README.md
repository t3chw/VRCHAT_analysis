# VRChat retention from Steam reviews

How long do people keep playing VRChat after they review it on Steam, who leaves sooner, and how would you test help that might keep them playing? This repository holds the data and the three notebooks behind a three part case study. Each notebook reproduces every result in its part, in the same order, and its section numbers follow the page.

## Notebooks

| Notebook | Case study part | What it covers |
|---|---|---|
| `VRChat_1_who_stays.ipynb` | Part 1: Who stays | The data; three measurement decisions (inputs known when the clock starts, a departure confirmed by a year without play, the clock at the review's last edit); the Kaplan Meier curve; who keeps playing by playtime, thumbs and language; the July 2022 protest week |
| `VRChat_2_who_leaves_sooner.ipynb` | Part 2: Who leaves sooner | The daily risk of leaving over time; the shift in 2024 and 2025; Cox models, a check of proportional hazards by time window, and one model per playtime band; tests on a held out half and on later years; expected retained days for six kinds of reviewer |
| `VRChat_3_designing_a_test.ipynb` | Part 3: A fair test | A randomised test of help for new players at a negative review: who to test and when, sample sizes, how long it would take, a simulation check of the power, what success would be worth, and the protest wave rule |

Run them in order: notebook 1 saves the retention table that notebooks 2 and 3 read. Notebook 2's Cox fits take a few minutes; the others run in seconds.

## Data

`data/vrchat_steam_reviews.csv.gz` holds every public Steam review of VRChat (app 438100): 267,904 reviews from 1 Feb 2017 to 28 Aug 2026, downloaded on 28 Aug 2026 from Steam's public review feed, `https://store.steampowered.com/appreviews/438100`.

- Review text, developer replies and review IDs are removed, and Steam IDs, names and profile links were never saved. Hardware details are reduced to a yes or no flag, `has_hardware`.
- One row per review. Steam allows one review per account and game, so each row is one reviewer.
- A review records the hours of play at the review and the last time Steam saw the reviewer play. It does not record when someone started playing.
- Timestamps are Unix seconds (UTC); playtimes are in minutes.
- Keep the row order: the held out split and the simulations depend on it.

The analysis uses `timestamp_created`, `timestamp_updated`, `author_last_played`, `author_playtime_at_review`, `voted_up` and `language`. The other columns stay in the file so notebook 1 can show why each one is dropped.

`data/retention_table.csv` is written by notebook 1 and not tracked in git.

## Setup

```bash
pip install pandas numpy scipy matplotlib lifelines jupyter
jupyter notebook
```

Tested with Python 3.12, pandas 3.0.5, numpy 2.5.2, scipy 1.18.1, lifelines 0.30.0 and matplotlib 3.11.1.

## Key definitions

- **Time zero:** the review's last edit, which is the posting day for the 82% of reviews never edited.
- **Departure:** the last observed session, counted only when a full year without play follows it. The analysis ends on 28 Aug 2025, a year before the download; the final year only confirms silence.
- **Already gone:** the last session came before time zero; counted as a departure at day 0.
- **Still playing:** seen playing after 28 Aug 2025; censored at that date.
- **Retained days:** calendar days from the review to the last observed session, capped at the horizon. They measure how long people stay, not how many days they play.

## Limits

- The data cover Steam reviewers only, and playing means playing through Steam.
- The results in Parts 1 and 2 are associations, not causes.
- Part 3 designs a test; its simulated readout is an illustration, not a result.
