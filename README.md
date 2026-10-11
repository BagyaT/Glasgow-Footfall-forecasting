# Glasgow City-Centre Footfall Forecasting

Forecasting daily pedestrian footfall across seven Glasgow city-centre locations using multivariate time-series modelling with weather and public-holiday data.

MSc Artificial Intelligence dissertation, University of Stirling (2022).

## Overview

Footfall is one of the clearest signals of how a city centre is performing. Retailers use it to plan staffing and stock, and councils use it to plan events, transport and investment. This project combines pedestrian counter data with weather and UK holiday data to understand what drives footfall in Glasgow and to forecast it ahead of time.

## Data

| Source | Description |
|---|---|
| Pedestrian counters | Footfall counts from 7 city-centre sensors (Argyll St, Buchanan St x2, Sauchiehall St x2, Bath St, High St), 2018–2021, ~220,000 records |
| Weather ([Meteostat](https://dev.meteostat.net/)) | Average, minimum and maximum temperature, wind speed and weather condition codes |
| UK public holidays | Bank holidays from 2018 to 2022 |

The three sources were cleaned and merged into a single dataset at location and date level.

## Approach

**1. Exploratory analysis**
- Footfall patterns by hour, weekday, week, month and year for each location
- Effect of temperature, weather conditions and holidays on footfall

**2. Time-series analysis**
- Seasonal decomposition (additive and multiplicative) to separate trend, seasonality and noise
- Augmented Dickey-Fuller (ADF) test to confirm stationarity (p < 0.001)

**3. Forecasting with Prophet**
- Log-transformed target to model multiplicative growth and stabilise variance
- Daily, weekly and yearly seasonality
- UK public holidays as special events
- Weather variables (temperature, min/max temperature, wind speed) as external regressors

**4. Hyperparameter tuning**
- Grid search over 150 model configurations, covering seasonality mode, changepoint flexibility, number of changepoints and holiday strength
- Models compared using Mean Absolute Percentage Error (MAPE)

**Train/test split:** trained on January 2018 – January 2020, tested on February 2020, deliberately before the COVID-19 lockdowns disrupted normal patterns.

## Results

| Location | Model | MAPE |
|---|---|---|
| Argyll Street | Baseline Prophet with weather regressors | 25.8% |
| Argyll Street | Tuned Prophet (grid search) | 22.5% |
| Buchanan Street | Tuned Prophet, multiplicative seasonality | 11.5%* |

*Measured across the full training and test window.

Key findings:
- Footfall follows strong weekly and daily cycles, peaking on Saturdays and in the afternoon.
- Multiplicative seasonality consistently outperformed additive seasonality.
- Holidays and weather add useful signal, but their effect varies by street.
- Tuning reduced forecast error by about 13% on Argyll Street.

## Tech stack

Python, Pandas, NumPy, Prophet, statsmodels, scikit-learn, Matplotlib, Plotly, Jupyter

## Repository structure

```
├── Dissertation.ipynb          # Data processing, exploratory analysis, decomposition, stationarity tests
├── Footfall_Forecasting.ipynb  # Prophet models, tuning and evaluation
└── README.md
```

## Limitations and future work

- Evaluate with rolling-origin cross-validation over multiple periods, rather than a single test month
- Use weather forecasts instead of observed weather, so the model works for real future predictions
- Model the COVID-19 period and recovery as a structural change
- Compare against gradient boosting (LightGBM) and deep learning models (LSTM, NeuralProphet)
- Forecast at hourly level and across all locations together
- Deploy as a scheduled forecasting service with a dashboard

## Author

**Bagya Tamilmani**, MSc Artificial Intelligence, University of Stirling
[LinkedIn](https://linkedin.com/in/bagya-tamilmani)
