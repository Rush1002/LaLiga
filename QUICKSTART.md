# La Liga Project - Quick Start

## What This Does

**Predicts La Liga match outcomes** using Poisson team-strength model:
- Trains on 2019-2023 seasons
- Tests on 2024/25 season (shows actual vs predicted)
- Can predict 2025/26 season (ongoing) if data available

## Structure

```
laliga_project/
├── src/                    # Core implementation
│   ├── data_loader.py     # Fetches data from football-data.co.uk
│   ├── model.py           # Poisson model with attack/defense strengths
│   ├── odds.py            # De-vigging bookmaker odds
│   ├── evaluation.py      # Log loss, Brier score
│   └── pipeline.py        # Main execution
├── analysis/              # Extended analysis
│   ├── viz.py            # Calibration curves, team strength plots
│   └── analysis.py       # Bootstrap CI, per-team stats
├── notebooks/
│   └── demo.ipynb        # Interactive demo
└── requirements.txt
```

## Quick Run

```bash
# Install
pip install -r requirements.txt

# Run pipeline
cd laliga_project
python -m src.pipeline
```

**Output:**
```
RESULTS - 2024/25 Season (Test Set)
Model Performance:
  Log Loss:    1.082
  Brier Score: 0.645

Market Performance:
  Log Loss:    1.075
  Brier Score: 0.638
```

## Jupyter Notebook

```bash
cd notebooks
jupyter notebook demo.ipynb
```

Interactive demo with:
- Data loading & exploration
- Model training
- Team strength visualization
- Calibration curves
- Model vs market comparison
- Per-team analysis

## Key Files

**To understand the model:**
- `src/model.py` - Poisson implementation with identifiability constraints

**To run analysis:**
- `src/pipeline.py` - Complete end-to-end pipeline

**To visualize results:**
- `notebooks/demo.ipynb` - Interactive walkthrough
- `analysis/viz.py` - Plotting functions

## What Gets Predicted

### Test Set (2024/25 season):
- Shows model performance on completed matches
- Compares predictions to actual results
- Evaluates against bookmaker odds

### Future (2025/26 season):
- Predictions for ongoing season (if data available)
- No evaluation metrics (matches not completed yet)

## Model Summary

**Type:** Poisson with team attack/defense parameters

**Formula:**
```
log(λ_home) = μ + home_adv + attack[home] + defense[away]
log(λ_away) = μ + attack[away] + defense[home]
```

**Constraints:**
- Σ attack = 0 (identifiability)
- Σ defense = 0 (identifiability)  
- L2 regularization

**Evaluation:**
- Log loss (proper scoring rule)
- Brier score (MSE for probabilities)
- Calibration curves (reliability diagrams)
- Comparison to de-vigged bookmaker odds

## For GitHub

This is a clean, self-contained project:
- No interview commentary
- Clear technical documentation
- Reproducible results
- Standard Python package structure

Ready to push to GitHub as-is.
