# MLB-InnBet
An algorithmic baseball analytics engine built to isolate and evaluate betting trends across specific game frames (Innings 2–6). The system dynamically fetches live game data via the MLB Stats API, builds historical snapshots, and simulates seasonal rolling backtests to optimize predictive weights.

## 📌 Project Overview
Unlike traditional full-game or first-inning (NRFI/YRFI) models, this project isolates the middle-inning "meat" of the game where starting pitchers face lineups for the 2nd/3rd time and bullpens begin to emerge. 

The core objective is to run chronological backtests to optimize predictive parameters, discovering the most profitable range of coefficients when adjusting a team's scoring averages against their opponent's rolling defensive/stadium strength.

### Key Features
* **Granular Data Pipeline:** Custom scripts to pull and map inning-by-inning performance ($Innings\ 2-6$).
* **Rolling Point-in-Time Backtester:** Simulates past days of the season chronologically using *only* the data available on that specific morning to prevent future-data leakage.
* **Strength-of-Schedule Weighting:** Adjusts team offensive averages dynamically based on opponent Home/Away win percentages.
* **Forthcoming GUI Dashboard:** A visual control panel to easily tweak model thresholds, input custom weights, and view daily action items.

---

## 🗂 Repository Structure
```text
├── data/
│   └── mlb_initial_load.json    # Local master database of season history
├── scripts/
│   ├── initial_load.py          # Builds/rebuilds the master JSON from scratch
│   ├── update.py                # Daily pipeline to pull the latest final box scores
│   └── backtester.py            # Chronological simulation engine for weight optimization
├── main.py                      # Daily analysis script to generate betting action
├── requirements.txt             # Project dependencies (MLB-StatsAPI, pandas, etc.)
└── README.md
