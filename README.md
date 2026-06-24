
from pathlib import Path

README_TEXT = r"""# RL_StatArb_Replication_Russell3000

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20570799.svg)](https://doi.org/10.5281/zenodo.20570799)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Replication](https://img.shields.io/badge/Replication-Public%20Safe-brightgreen)
![Bloomberg Data](https://img.shields.io/badge/Bloomberg%20Data-Not%20Redistributed-red)
![Status](https://img.shields.io/badge/Status-Research%20Replication%20Package-purple)

## Replication Package for

# The Limits of Reinforcement Learning in Statistical Arbitrage  
## A Large-Universe Benchmark Against Classical Mean Reversion

**Author:** Veliota Drakopoulou  
**Affiliations:**  
Higher Colleges of Technology, United Arab Emirates  
Embry-Riddle Aeronautical University, United States  

**ORCID:** [0000-0002-1670-8033](https://orcid.org/0000-0002-1670-8033)  
**Contact:** vdrakopoulou@hct.ac.ae  

---

## Overview

This repository provides the public-safe replication package for the paper:

> **The Limits of Reinforcement Learning in Statistical Arbitrage: A Large-Universe Benchmark Against Classical Mean Reversion**

The study evaluates whether standard reinforcement-learning agents can improve statistical-arbitrage performance when tested against a strong classical benchmark in a large and dynamically selected equity universe.

Using Bloomberg Russell 3000 data from 2016 to 2025, the paper constructs a walk-forward pairs-trading environment with:

- sector-aware candidate formation,
- correlation screening,
- cointegration testing,
- half-life estimation,
- liquidity filtering,
- transaction costs,
- market-regime variables,
- rolling walk-forward validation.

The empirical framework compares a transparent classical **z-score mean-reversion benchmark** with four reinforcement-learning algorithms:

| Model | Action space | Empirical role |
|---|---|---|
| Z-Score | Rule-based flat / long / short | Classical mean-reversion benchmark |
| PPO | Discrete flat / long / short | Learned trade timing |
| A2C | Discrete flat / long / short | Learned trade timing |
| A2C Directional | Discrete flat / long / short | Reward-shaped exposure discipline |
| SAC | Continuous exposure in [-1, +1] | Learned position sizing |
| TD3 | Continuous exposure in [-1, +1] | Deterministic position sizing |

The continuous-action agents learn target spread exposure directly:

```text
-1 = full short spread
 0 = flat
+1 = full long spread
```

---

## Main Finding

The results show that greater algorithmic flexibility does **not** automatically improve statistical-arbitrage performance.

The classical z-score benchmark achieves the strongest average risk-adjusted results, producing the highest Sharpe, Sortino, and Calmar ratios across the walk-forward test windows.

Key empirical findings:

- PPO and A2C learn active trading policies but remain overexposed.
- Directional reward shaping reduces A2C's active rate from **96.1%** to **33.5%** and improves drawdown control.
- SAC and TD3 underperform despite their ability to adjust position size continuously.
- Continuous action spaces do not solve the reinforcement-learning performance gap.
- Standard reinforcement-learning agents require stronger portfolio-level risk control before they can reliably improve on transparent mean-reversion rules.

The central conclusion is deliberately conservative:

> **In large-universe statistical arbitrage, model complexity does not guarantee superior trading performance.**

---

## Public-Safe Code Pipeline

This repository includes the full public-safe code pipeline from Bloomberg data collection to model training, algorithm comparison, and statistical validation.

```text
01 Build Russell 3000 ticker list from Bloomberg export
02 Download Bloomberg OHLCV and market-cap data
03 Download market-regime data
04 Download Bloomberg metadata
05 Clean/filter stock panel and build tradable universe
06 Dynamic pair selection
07 Build RL pair dataset
08 Train PPO and A2C
09 Train directional reward-shaped A2C
10 Train SAC continuous-action model
11 Train TD3 continuous-action model
12 Compare all algorithms
13 Run statistical tests
```

The code implements a common empirical framework so that all models are evaluated using the same data environment, dynamic pair-selection process, transaction-cost assumption, and walk-forward validation protocol.

---

## Repository Structure

```text
RL_StatArb_Replication_Russell3000/
|
|-- README.md
|-- LICENSE
|-- CITATION.cff
|-- requirements.txt
|-- environment.yml
|
|-- code/
|   |-- 01_build_tickers.py
|   |-- 02_download_russell3000_ohlcv.py
|   |-- 03_download_regime_data.py
|   |-- 04_download_metadata.py
|   |-- 05_clean_filter_universe.py
|   |-- 06_dynamic_pair_selection.py
|   |-- 07_build_rl_pair_dataset.py
|   |-- 08_train_ppo_a2c.py
|   |-- 09_directional_reward_a2c.py
|   |-- 10_train_sac_continuous.py
|   |-- 11_train_td3_continuous.py
|   |-- 12_compare_all_algorithms.py
|   |-- 13_statistical_tests.py
|
|-- docs/
|   |-- DATA_AVAILABILITY.md
|   |-- BLOOMBERG_DATA_RESTRICTION_NOTICE.md
|   |-- RUN_ORDER.md
|   |-- METHODOLOGY_SUMMARY.md
|   |-- RESULTS_INTERPRETATION.md
|
|-- data/
|   |-- aggregate_results/
|       |-- rolling_walkforward_rl_results.csv
|       |-- reward_engineering_results.csv
|       |-- directional_reward_results.csv
|       |-- sac_continuous_results.csv
|       |-- td3_continuous_results.csv
|       |-- all_algorithm_results_combined.csv
|
|-- tables/
|   |-- main/
|   |-- statistical_tests/
|
|-- figures/
|   |-- average_sharpe_by_model.png
|   |-- risk_return_profile.png
|   |-- cumulative_wealth_curves.png
|   |-- active_rate_by_model.png
|   |-- source_data/
|
|-- replication_metadata/
    |-- file_manifest.txt
    |-- file_manifest_sha256.csv
```

---

## Bloomberg Data Restriction

This study uses Bloomberg-derived data, including:

```text
Russell 3000 equity OHLCV data
Market capitalization data
Sector and industry classification data
S&P 500 market data
VIX data
U.S. Treasury yield data
Bloomberg Russell 3000 membership information
```

Due to Bloomberg and institutional data-licensing restrictions, the raw and processed Bloomberg-derived datasets are **not redistributed** in this public repository.

This repository does **not** include:

```text
Raw Bloomberg OHLCV files
Processed Parquet datasets
Russell 3000 membership exports
Bloomberg GICS metadata exports
Market-regime data files derived from Bloomberg
Trained models derived from Bloomberg data
Detailed trade logs
Daily return logs
```

Researchers with appropriate Bloomberg access may reproduce the dataset by running the provided data-collection and preprocessing scripts in the documented order.

---

## Installation

Create a clean Python environment:

```bash
python -m venv rl_stat_arb_env
```

Activate it.

### Windows

```bash
rl_stat_arb_env\Scripts\activate
```

### macOS / Linux

```bash
source rl_stat_arb_env/bin/activate
```

Install requirements:

```bash
pip install -r requirements.txt
```

Required packages include:

```text
pandas
numpy
pyarrow
openpyxl
xbbg
statsmodels
scikit-learn
gymnasium
stable-baselines3
torch
joblib
matplotlib
scipy
```

---

## Bloomberg Requirements

To reproduce the full dataset, the user must have:

```text
Bloomberg Terminal or B-PIPE access
Bloomberg Desktop API installed
xbbg configured correctly
Valid Bloomberg data entitlements
```

A basic Bloomberg connectivity test is:

```python
from xbbg import blp

print(blp.bdp("AAPL US Equity", "PX_LAST"))
```

If this returns a price, Bloomberg connectivity is working.

---

## Run Order

Run scripts from the repository root.

```bash
cd RL_StatArb_Replication_Russell3000
```

### 1. Data Collection

```bash
python code/01_build_tickers.py
python code/02_download_russell3000_ohlcv.py
python code/03_download_regime_data.py
python code/04_download_metadata.py
```

### 2. Data Cleaning and Universe Construction

```bash
python code/05_clean_filter_universe.py
```

### 3. Dynamic Pair Selection

```bash
python code/06_dynamic_pair_selection.py
```

### 4. RL Dataset Construction

```bash
python code/07_build_rl_pair_dataset.py
```

### 5. Baseline PPO and A2C Training

```bash
python code/08_train_ppo_a2c.py
```

### 6. Directional Reward-Shaped A2C

```bash
python code/09_directional_reward_a2c.py
```

### 7. Continuous-Action SAC

```bash
python code/10_train_sac_continuous.py
```

### 8. Continuous-Action TD3

```bash
python code/11_train_td3_continuous.py
```

### 9. Algorithm Comparison

```bash
python code/12_compare_all_algorithms.py
```

### 10. Statistical Tests

```bash
python code/13_statistical_tests.py
```

---

## Model Families

The empirical design evaluates four model families.

| Family | Model variants | Purpose |
|---|---|---|
| Classical benchmark | Z-Score | Transparent mean-reversion benchmark |
| Discrete-action RL | PPO, A2C | Tests learned long / short / flat trade timing |
| Reward-shaped RL | A2C directional | Tests whether reward shaping reduces overexposure |
| Continuous-action RL | SAC, TD3 | Tests whether position sizing solves the action-space limitation |

This design tests three possible reasons why reinforcement learning may underperform:

```text
1. Poor trade timing
2. Reward misspecification
3. Restrictive action-space design
```

---

## Key Results

The final all-algorithm comparison shows that the classical z-score benchmark remains the strongest model on average.

| Model variant | Main finding |
|---|---|
| Z-Score | Highest average Sharpe, Sortino, and Calmar ratios |
| PPO | Positive return but high active rate and weak Sharpe |
| A2C | Better than PPO but still below z-score |
| A2C Directional | Much lower exposure but does not close performance gap |
| SAC | Negative average Sharpe despite continuous position sizing |
| TD3 | Negative average Sharpe and high exposure |

The results suggest that reinforcement learning does not automatically improve statistical-arbitrage performance in a large, dynamically selected equity universe.

---

## Replication Level

This repository supports **public replication** of:

```text
research pipeline
model definitions
run order
aggregate empirical results
statistical-test summaries
figures and tables
```

It does not support direct rerunning of the full experiment without licensed Bloomberg access because the public repository does not contain raw or processed Bloomberg-derived data.

---

## Zenodo DOI

This replication package is archived on Zenodo:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20570799.svg)](https://doi.org/10.5281/zenodo.20570799)

---

## Citation

If you use this repository, please cite:

```bibtex
@software{drakopoulou_2026_rl_stat_arb,
  author       = {Drakopoulou, Veliota},
  title        = {RL_StatArb_Replication_Russell3000: Replication Package for The Limits of Reinforcement Learning in Statistical Arbitrage},
  year         = {2026},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.20570799},
  url          = {https://doi.org/10.5281/zenodo.20570799}
}
```

---

## License

Code is released under the MIT License.

Documentation, aggregate tables, and figures are provided for academic reuse with attribution.

Raw and processed Bloomberg-derived data are not included and remain subject to Bloomberg and institutional licensing restrictions.

---

## Contact

**Veliota Drakopoulou**  
Higher Colleges of Technology, United Arab Emirates  
Embry-Riddle Aeronautical University, United States  

ORCID: [0000-0002-1670-8033](https://orcid.org/0000-0002-1670-8033)  
Email: vdrakopoulou@hct.ac.ae  

---

## Acknowledgment

This repository was prepared to support transparent, public-safe replication of an empirical study on AI-based statistical arbitrage. The research uses Bloomberg data locally but does not redistribute Bloomberg-derived datasets.

---

## Disclaimer

This repository is provided for academic research and reproducibility purposes only. It is not investment advice, trading advice, or a recommendation to buy or sell any security. Past backtest performance does not imply future trading performance.
"""


def main() -> None:
    output_path = Path("README.md")
    output_path.write_text(README_TEXT, encoding="utf-8")
    print(f"Created {output_path.resolve()}")


if __name__ == "__main__":
    main()
