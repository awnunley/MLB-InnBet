# MLB-InnBet
## MLB Quantitative Analytics & Predictive Inning Engine
A decoupled desktop application and backtesting framework that parses live MLB data streams to isolate situational performance splits and compute portfolio stake sizes using the Kelly Criterion framework.

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
```

## ⚙️ The Mathematical & Predictive Logic

The core calculation normalizes a team's raw scoring average per inning by evaluating their opponent's specific environmental performance.

### Step 1: Point-in-Time Statistics
For any simulated date $D$, the model calculates a team's rolling average for an inning ($I$) and the opponent's venue-specific win percentage using *only* games played prior to date $D$.

### Step 2: Weighting for Matchup Difficulty
To account for strength of schedule, raw scoring averages are adjusted using the opponent’s Home or Away win percentage ($WP_{opp}$):

$$\text{Weighted Score} = \text{Raw Inning Avg} \times \left(1 + \left(WP_{opp} - 0.5\right)\right)$$

* **Tough Road Opponent ($WP_{opp} = 0.600$):** Grants a $+10\%$ boost to the scoring projection, valuing runs earned against premier teams higher.
* **Weak Road Opponent ($WP_{opp} = 0.350$):** Applies a $-15\%$ penalty, deflating stats padded against weaker teams.

---

## 🚀 Getting Started

### 1. Installation & Environment Setup
Clone the repository and install the necessary data science libraries:
```bash
git clone [https://github.com/YOUR_USERNAME/mlb-inning-backtester.git](https://github.com/YOUR_USERNAME/mlb-inning-backtester.git)
cd mlb-inning-backtester
pip install -r requirements.txt
