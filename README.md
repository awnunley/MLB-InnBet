# MLB Quantitative Analytics & Predictive Inning Engine
A decoupled desktop application and backtesting framework that parses live MLB data streams to isolate situational performance splits and compute portfolio stake sizes using the Kelly Criterion framework.

## 📌 Project Overview
Unlike traditional full-game or first-inning (NRFI/YRFI) models, this project isolates the middle-inning "meat" of the game where starting pitchers face lineups for the 2nd/3rd time and bullpens begin to emerge. 

The core objective is to run chronological backtests to optimize predictive parameters, discovering the most profitable range of coefficients when adjusting a team's scoring averages against their opponent's rolling defensive/stadium strength.


## Features

* **Real-Time Data Ingestion:** Automates live daily game slate pulling and hydrates active roster matchups directly through the MLB stats API wrapper.
* **Asynchronous Execution UI:** Background threading worker prevents application lockups, maintaining a responsive user interface during network-heavy I/O operations.
* **Dynamic Parameterized Backtesting:** Features a historical grid-search simulation engine to evaluate and isolate high-performing operational split thresholds.
* **Automated Capital Allocation:** Built-in risk-mitigation framework running fractional Kelly Criterion calculations to safeguard portfolio management.

### Application Preview
![Alt text](Preview.png?raw=true "Preview")

## System Architecture
### 1. Data Pipeline & Snapshot Matrix (`OverUnder2.py`)
The core processing script functions as an automated data pipeline that operates directly on live data frames and memory streams:
* **Dynamic Platoon Split Compilation:** Instead of relying on static, flat-rate season averages, the engine dynamically builds situational splits (e.g., Target Inning performance isolated by facing RHP vs LHP) directly from the master dataset (`mlb_initial_load.json`).
* **Live Ingest & Hydration:** Utilizes `statsapi` to query the daily schedule payload, programmatically extracting upcoming matchups and hydrating individual starting pitcher profiles to pull throw-hand codes (`L` / `R`) in real-time.
* **Quant Matrix Formula:** Implements a parameterized weighting matrix that adjusts raw team averages against opponent win percentages using strict, hyperparameter-optimized thresholds ($Crit > 0.75$ and $Crit < 0.25$) verified during historical backtesting.
* **Risk Management Engine:** Automatically passes calculated win probabilities into a half-Kelly criterion allocation algorithm to generate defensive stake sizing recommendations based on target user bankrolls and real-time bookmaker decimal odds multipliers.

### 2. Presentation Layer & Concurrency Engine (`dashboard.py`)
The user interface is built as an asynchronous desktop dashboard utilizing Python’s native structural layout patterns:
* **Asynchronous Multithreading:** To avoid the common pitfall of a frozen GUI during high-latency network requests, the interface isolates live API scraping and calculations to a dedicated background worker thread (`threading.Thread`).
* **Thread-Safe Presentation Intercept:** The worker thread securely schedules UI updates back to the main thread via Tkinter's safe scheduling loop (`root.after`), preventing thread race conditions and ensuring deterministic UI element updates.
* **State-Driven Treeview & Styling:** Leverages `ttk.Treeview` spreadsheet elements and hierarchical style maps to dynamically intercept calculations. It applies distinct CSS-like hexadecimal tags to visually isolate active edge plays (soft green) from dead matchups, making operational data immediately actionable.

### 3. Hyperparameter Optimization & Backtester (`backtest.py`)
The "training" and verification ground of the algorithm acts as a historical simulation sandbox:
* **Combinatorial Hyperparameter Grid Search:** Employs nested iterative loops (`itertools.product`) to test combinations of performance weights across historical spans.
* **Algorithmic Validation:** Simulates past game scenarios to cross-examine what the mathematical matrix *would* have recommended against the actual boxscore outcomes, protecting model integrity and isolating highly profitable calculation sub-windows.

---

## Getting Started

### 1. Installation & Environment Setup
Clone the repository and install the necessary data science libraries:
```bash
git clone [https://github.com/YOUR_USERNAME/mlb-inning-backtester.git](https://github.com/YOUR_USERNAME/mlb-inning-backtester.git)
cd mlb-inning-backtester
pip install -r requirements.txt
