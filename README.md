# Daily Temperature Forecasting using Classical Time Series Models

## Overview
This project compares classical statistical forecasting models for predicting daily average temperature at the Thiruvananthapuram weather station using approximately ten years of daily observations.

## Dataset
- Source: India Meteorological Department (IMD)
- Station: Thiruvananthapuram
- Frequency: Daily
- Duration: ~10 years

## Workflow
- Data Cleaning & Preprocessing
- Exploratory Data Analysis
- Time Series Diagnostics
- Train / Validation / Test Split
- Seasonal Naïve
- Seasonal Moving Average
- Fourier Regression
- Residual Diagnostics
- ARIMA
- Model Comparison

## Models Compared
- Seasonal Naïve
- Seasonal Moving Average
- Fourier Regression
- ARIMA

## Evaluation Metrics
- MAE
- RMSE
- sMAPE
- MASE

## Key Findings
- Strong annual seasonality was observed.
- Fourier Regression produced the best overall forecasting performance.
- ARIMA improved upon baseline models but did not outperform Fourier Regression.

## Repository Structure
```
.
├── temperature_forecasting.ipynb
├── PROJECT_FLOW.md
├── README.md
└── requirements.txt
```

## Requirements
- Python 3.x
- pandas
- numpy
- matplotlib
- statsmodels
- scikit-learn

## Future Work
- Rolling-origin validation
- Dynamic Harmonic Regression
- Prediction intervals
