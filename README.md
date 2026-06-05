# Explainable RegTech Early-Warning Framework for Meme-Stock Market Disruption

## Overview
This repository contains the replication code for the paper:
**"Explainable RegTech for Social Media Driven Market Disruption: 
An Early Warning Framework for Meme Stock Surveillance"**
University of Birmingham, 2025

## Repository Structure
paper1-meme-stock-ews/
├── during the GameStop/              # GME main analysis
│   ├── 1. data/                      # Data pipeline
│   │   ├── gme stock data/           # Market data & EWS labels
│   │   ├── reddit comments and posts/ # Reddit data & cascade features
│   │   ├── network features/         # Network, temporal & text features
│   │   ├── coordination features/    # Burstiness, overlap & duplication
│   │   ├── merge all feature/        # Feature merging & scaling
│   │   └── word cloud/               # Visualisation
│   ├── 2. correlation check/         # Leakage screening
│   ├── 3. models/                    # Model training (5/15/30min)
│   │   ├── model and visual for 5 minutes/
│   │   ├── model and visual for 15 minutes/
│   │   └── model and visual for 30 minutes/
│   └── 4. robustness/                # Six robustness checks
│       ├── Calibration/
│       ├── Placebo test/
│       ├── rolling validation/
│       └── workspace/                # Alternative label configs
│
├── AMC/                              # AMC external validation
│   ├── 1. data/                      # AMC data pipeline
│   ├── 2. preprocessing/             # Feature verification
│   └── 3. models/                    # AMC prediction
│
└── Panel logit/                      # Stata logit analysis
├── Export for stata.ipynb        # Data export
└── stata_data/                   # Exported CSV files

## Data Sources
- **Market data**: GME and AMC 5-minute OHLCV bars from Alpaca Markets
  (July 2019 – June 2021)
- **Social media**: r/wallstreetbets archived posts and comments
  from Academic Torrents
- **Trading halts**: NYSE official halt records

> Raw data files (.parquet, .csv, .zst) are excluded from this
> repository. Contact the author for data access instructions.

## Requirements
Python 3.11
lightgbm
xgboost
scikit-learn
pandas
numpy
torch
shap
matplotlib
networkx
pyarrow
statsmodels

Install:
```bash
pip install lightgbm xgboost scikit-learn pandas numpy shap matplotlib networkx pyarrow statsmodels
```

## Execution Order

**Step 1: GME Data Pipeline**
during the GameStop/1. data/gme stock data/
├── financial data download.ipynb    # Download market data
└── clean labels.ipynb               # Construct EWS labels

**Step 2: Feature Engineering**
during the GameStop/1. data/
├── reddit comments and posts/zst_processor.ipynb
├── reddit comments and posts/cascade feature.ipynb
├── network features/network features.ipynb
├── network features/temporal features.ipynb
├── network features/NLP Feature.ipynb
├── coordination features/Extract burstiness features.ipynb
├── coordination features/Extract user overlap features.ipynb
├── coordination features/Extract text duplication features precise.ipynb
└── merge all feature/Merge all features.ipynb

**Step 3: Model Training**
during the GameStop/3. models/model and visual for 15 minutes/data/
├── lightgbm.ipynb      # Primary model (LightGBM)
├── xgboost.ipynb       # Comparison model
├── Logistic.ipynb      # Linear baseline
└── CNN-GRU.ipynb       # Deep learning baseline

**Step 4: Robustness Checks**
during the GameStop/4. robustness/
├── run_robustness.ipynb             # Alternative label definitions
├── Placebo test/placebo_test.ipynb  # Placebo tests
├── Calibration/Calibration test.ipynb
└── rolling validation/Rolling validation.ipynb

**Step 5: AMC External Validation**
AMC/1. data/                         # AMC data pipeline (same as GME)
AMC/3. models/model and visual for 15 minutes/
└── Lightgbm with save.ipynb         # Load GME model, predict AMC

**Step 6: Panel Logit (Stata)**
Panel logit/Export for stata.ipynb   # Export data to CSV
Panel logit/panel_logit.do           # Run in Stata

## Key Results
| Model | EWS@5min | EWS@15min | EWS@30min |
|---|---|---|---|
| LightGBM | 0.646 | 0.546 | 0.451 |
| XGBoost | 0.589 | 0.489 | 0.440 |
| Logistic | 0.308 | 0.355 | 0.407 |
| CNN-GRU | 0.270 | 0.312 | 0.276 |
| Random Baseline | 0.067 | 0.105 | 0.133 |

AMC External Validation: PR-AUC = 0.341 (8.1× over random baseline)

## Citation
> Deng, D. (2025). Explainable RegTech for Social Media Driven 
> Market Disruption: An Early Warning Framework for Meme Stock 
> Surveillance. Working Paper, University of Birmingham.

## License
This repository is for academic research purposes only.
Raw data redistribution is subject to original source terms
(Alpaca Markets API, Academic Torrents, NYSE).