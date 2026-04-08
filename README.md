# The Endless Line — Forecasting Theme Park Wait Times

> A data-driven approach to predicting attraction waiting times at PortAventura World, turning 3.5M+ data points into actionable 15-minute granularity forecasts.

---

## Context

Post-COVID, theme park visitors face average waits of **23 minutes** — with peaks exceeding **60 minutes** at top attractions. Park management lacks predictive tools and relies on reactive staffing and static scheduling.

This project tackles two questions:
1. **How to accurately forecast attraction wait times** days/weeks ahead under normal operations?
2. **How to turn those forecasts into operational decisions** — dynamic staffing, visitor redistribution, and revenue optimization?

## Why This Problem Is Hard

- **High dimensionality**: 26 attractions × 15-min intervals × 5 years = 3.5M+ raw data points
- **Non-stationarity**: Post-COVID behavioral shifts mean historical patterns don't directly apply
- **Exogenous factors**: Weather, holidays, parades, school vacations all influence demand non-linearly
- **Per-attraction variation**: Each ride has unique demand profiles — no one-size-fits-all model works
- **Data quality**: Rehabilitation periods, breakdowns, and capacity changes create noise

## Approach

### Data Pipeline

| Stage | What we did |
|-------|-------------|
| **Ingestion & Cleaning** | Merged 6 datasets. Filtered rehab periods, breakdowns, zero-capacity records. 3.5M → 2M clean rows |
| **Feature Engineering** | 54 initial features → 8 final features including temporal encodings, weather, attendance, capacity utilization, and target encoding at 15-min granularity |
| **Validation** | Expanding-window cross-validation (5 folds, 7-day test windows) |
| **Evaluation** | MAE/RMSE on held-out week. Per-attraction model instances (×23) |

### Selected Features

| Type | Feature | Why |
|------|---------|-----|
| Engineered | Typical Wait Profile | Target-encoded avg wait by attraction, day of week, and 15-min block |
| Demand | Attendance | Daily park attendance — strongest demand proxy |
| Operational | Open Time / Up Time | Scheduled vs. actual operating time |
| Weather | Temperature, Rain (1h) | Outdoor ride demand varies with conditions |
| Temporal | Month, Day Category | Seasonal patterns + weekday/weekend/holiday encoding |

### Models Benchmarked

We trained and compared three gradient boosting frameworks — the gold standard for structured tabular data:

- **XGBoost**
- **LightGBM**
- **CatBoost**

Each model was trained as **per-attraction instances** (23 separate models) to capture ride-specific demand patterns.

## Key EDA Insights

- Peak waiting hours occur between **12:00–15:00** with a clear bell-curve pattern
- **Saturday** is the busiest day (~31 min avg) vs. mid-week (~17 min)
- Top 3 attractions average **47–57 min** wait times — requiring dedicated optimization

## Tech Stack

`Python` · `XGBoost` · `LightGBM` · `CatBoost` · `Pandas` · `Scikit-learn` · `Matplotlib` · `Seaborn`

## Team

- Nicolas Perion — ML Engineering, Strategy
- **Marta Shkreli** — Data Processing, EDA
- Anas Khalil — Pipeline, Data Quality
- Zihan Yang — Model Development
- Neil Dinia — KPI Strategy, Consulting
- Matteo Couchoud — Time-Series, Feature Engineering

MSc in Data Science & Business Analytics — ESSEC & CentraleSupélec (2025–2026)
