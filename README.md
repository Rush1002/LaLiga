# La Liga Match Outcome Prediction

Statistical modeling of La Liga soccer matches using Poisson team-strength model. Compares model predictions to bookmaker odds using proper scoring rules.

## Overview

This project implements an interpretable probabilistic model for predicting soccer match outcomes:
- **Model**: Poisson distribution with team attack/defense strengths
- **Data**: Historical La Liga match results and bookmaker odds (2019-2025)
- **Evaluation**: Log loss, Brier score, calibration analysis

## Model Specification

Goals are modeled as Poisson random variables with team-specific parameters:

```
log(λ_home) = μ + home_advantage + attack[home_team] + defense[away_team]
log(λ_away) = μ + attack[away_team] + defense[home_team]
```

**Parameters:**
- `μ`: Base goal rate
- `home_advantage`: Home field effect (~0.1 on log scale)
- `attack[team]`: Team offensive strength
- `defense[team]`: Team defensive weakness

**Constraints:**
- Σ attack = 0 (identifiability)
- Σ defense = 0 (identifiability)
- L2 regularization on team parameters

## Project Structure

```
laliga_project/
├── src/
│   ├── data_loader.py    # Data fetching from football-data.co.uk
│   ├── model.py          # Poisson strength model implementation
│   ├── odds.py           # De-vigging utilities
│   ├── evaluation.py     # Scoring rules and metrics
│   └── pipeline.py       # End-to-end training and prediction
├── analysis/
│   ├── viz.py            # Visualization functions
│   └── analysis.py       # Extended analysis utilities
├── notebooks/
│   └── demo.ipynb        # Interactive demo
└── requirements.txt
```

## Installation

```bash
pip install -r requirements.txt
```

## Usage

### Basic Pipeline

```python
from src.pipeline import run

results = run()

# Access components
model = results['model']
test_predictions = results['test_df']
metrics = results['test_metrics']
```

### Command Line

```bash
python -m src.pipeline
```

Output:
```
RESULTS - 2024/25 Season (Test Set)
Model Performance:
  Log Loss:    1.082
  Brier Score: 0.645

Market Performance:
  Log Loss:    1.075
  Brier Score: 0.638
```

## Data Source

Historical match data from [football-data.co.uk](https://www.football-data.co.uk/):
- Match results (home/away goals)
- Bookmaker odds (averaged across multiple bookmakers)
- Seasons: 2019/20 through 2024/25

## Training/Test Split

- **Train**: 2019/20 - 2022/23 (4 seasons)
- **Validation**: 2023/24 (1 season)
- **Test**: 2024/25 (1 season) - used for evaluation
- **Future**: 2025/26 (ongoing) - predictions for current season

## Evaluation Metrics

**Log Loss (Cross-Entropy)**
```
LogLoss = -mean(log(p_predicted[true_outcome]))
```
Lower is better. Heavily penalizes confident wrong predictions.

**Brier Score**
```
Brier = mean((p_predicted - y_onehot)²)
```
Lower is better. Mean squared error for probabilities.

Both are proper scoring rules that incentivize honest probability estimates.

## Key Features

- **Interpretable Parameters**: Team strengths have clear meaning (deviation from league average)
- **Fair Comparison**: De-vigs bookmaker odds before evaluation (removes bookmaker margin)
- **Proper Validation**: Time-based split prevents data leakage
- **Regularization**: L2 penalty prevents overfitting on small samples

## Results Summary

The model achieves competitive performance with bookmaker probabilities:
- Model log loss within ~1% of market
- Strong correlation with market probabilities (r > 0.85)
- Calibration curves show reasonable reliability

Expected Value (EV) calculations are included for analysis but should not be interpreted as trading signals. Betting markets are highly efficient.

## Extending the Model

Potential improvements:
- Add player-level features (injuries, form)
- Model time-varying team strengths
- Include additional covariates (weather, rest days)
- Implement Bayesian posterior for parameter uncertainty
- Multi-league modeling for cross-validation

## Dependencies

- numpy >= 1.24.0
- pandas >= 2.0.0
- scipy >= 1.11.0
- scikit-learn >= 1.3.0
- matplotlib >= 3.7.0
- seaborn >= 0.12.0

## License

MIT
