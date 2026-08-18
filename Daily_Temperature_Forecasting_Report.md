# PROJECT REPORT
## Daily Temperature Forecasting using Classical Time Series Models

## 1. Project Overview

### Objective

The objective of this project was to forecast the daily average temperature at the Thiruvananthapuram weather station using approximately ten years of historical observations. Rather than focusing on a single forecasting model, the project aimed to compare different classical time series approaches and identify the model that best captures the underlying seasonal behaviour.

The project follows a complete forecasting pipeline consisting of data preprocessing, exploratory data analysis (EDA), statistical diagnostics, baseline model development, advanced time-series modelling, residual analysis and final model comparison.

### Dataset

**Source:** Indian Rainfall and Weather Data (Kaggle)

https://www.kaggle.com/datasets/ameydilipmorye/indian-rainfall-and-weather-data

The dataset contains daily weather observations collected from 458 weather stations across India between 2015 and 2025, including variables such as average temperature, rainfall, wind speed, air pressure, elevation and geographical coordinates.

Since multiple stations within a state experience different climatic conditions, a single station (Thiruvananthapuram) was selected to ensure the time series represented one consistent climatic process. The forecasting target was average daily temperature.

## 2. Project Workflow

The project followed the workflow below:

```
Raw Dataset
      ↓
Station Selection
      ↓
Data Cleaning
      ↓
Exploratory Data Analysis
      ↓
Time Series Diagnostics
      ↓
Chronological Train–Validation–Test Split
      ↓
Baseline Models
      ↓
Fourier Regression
      ↓
Residual Diagnostics
      ↓
ARIMA
      ↓
Model Comparison
      ↓
Final Conclusions
```

Unlike traditional statistical workflows, the project adopted a machine learning style evaluation strategy. A chronological train-test split was used to preserve temporal order, while a validation subset was created from the training data to tune the harmonic order of the Fourier Regression model. The test set remained untouched until final evaluation, preventing information leakage.

## 3. Data Preparation and Exploratory Analysis

### Data Cleaning

The following preprocessing steps were performed:

- Selected observations corresponding to the Thiruvananthapuram weather station.
- Retained only the required variables (`date_of_record` and `avg_temp`).
- Converted dates to datetime format and sorted the observations chronologically.
- Set the date column as the time-series index.
- Checked for duplicate observations.
- Checked for missing values.
- Verified continuity of the daily observations.

No duplicate or missing values were found after filtering, allowing the analysis to proceed without imputation.

### Exploratory Data Analysis

Several visualizations were created to understand the behaviour of the series:

- Daily temperature time-series plot
- Rolling mean and rolling standard deviation
- Monthly boxplots
- Monthly average temperature profile
- STL decomposition
- Autocorrelation (ACF)
- Partial autocorrelation (PACF)

### Key Findings

- The temperature series exhibited a strong annual seasonal pattern, with similar behaviour repeating each year.
- Long-term trend was minimal, indicating relatively stable climatic conditions over the study period.
- Variability remained nearly constant, suggesting stable variance.
- Monthly distributions clearly reflected seasonal transitions.
- STL decomposition confirmed that seasonality was the dominant component of the series.

These findings suggested that explicitly modelling annual seasonality would likely provide better forecasts than relying solely on autoregressive behaviour.

## 4. Time Series Diagnostics

Before developing forecasting models, the statistical properties of the series were examined.

### ACF and PACF

The ACF showed strong positive autocorrelation over multiple lags, indicating substantial temporal dependence arising from seasonal behaviour. The PACF suggested that only a few autoregressive terms would likely be required for a non-seasonal ARIMA model.

### Stationarity

The Augmented Dickey-Fuller (ADF) test was used to assess stationarity. Combined with the visual diagnostics, the series was found to be suitable for modelling after accounting for seasonal effects.

## 5. Model Development

### Train–Validation–Test Split

Instead of a random split, the data was divided chronologically:

- **Training set:** Model fitting
- **Validation set:** Selection of Fourier harmonic order
- **Testing set:** Final model evaluation

This approach mirrors real forecasting scenarios and prevents future information from influencing model development.

### Baseline Models

#### Seasonal Naïve

The Seasonal Naïve model forecasts each day using the observation from the same day in the previous year. Since the series displayed strong yearly seasonality, this served as the primary benchmark.

#### Seasonal Moving Average

A Seasonal Moving Average model was also implemented to evaluate whether averaging observations from neighbouring seasonal periods could improve forecast accuracy. Although slightly smoother, it performed marginally worse than the Seasonal Naïve model.

### Fourier Regression

Given the strong annual seasonality observed during EDA, Fourier Regression was selected as the primary modelling approach. Instead of using seasonal lags, the model represents annual seasonality through sine and cosine basis functions.

The number of harmonics (K) was treated as a hyperparameter. Different harmonic orders were evaluated using the validation subset, and the value producing the lowest validation error was selected. This prevented overfitting while ensuring that the seasonal pattern was represented adequately.

Fourier Regression achieved the best forecasting performance among all evaluated models, demonstrating that deterministic seasonal modelling was highly effective for this dataset.

### Residual Diagnostics

Residual analysis was performed to evaluate whether the Fourier model had successfully captured the seasonal structure.

The following diagnostics were examined:

- Residual time-series plot
- Residual ACF
- Durbin–Watson statistic
- Ljung–Box test

The residuals showed that most of the annual seasonal structure had been removed. However, the residual ACF, Durbin–Watson statistic and Ljung–Box test indicated the presence of some remaining short-term autocorrelation. This suggests that while Fourier Regression effectively modelled deterministic seasonality, a small amount of stochastic dependence remained unexplained.

### ARIMA

A non-seasonal ARIMA model was fitted to determine whether modelling temporal dependence alone could outperform deterministic seasonal modelling.

Model orders were selected using a grid search based on the Akaike Information Criterion (AIC). Although ARIMA improved upon the baseline models, it did not outperform Fourier Regression. This indicates that the dominant source of predictability in the series is the annual seasonal cycle rather than short-term autoregressive dependence.

Classical SARIMA models with a yearly seasonal period were considered but not included in the final comparison because fitting seasonal models with a period of 365 days was computationally expensive. Since Fourier Regression already provided an efficient representation of long seasonal cycles, additional SARIMA experimentation was not pursued.

## 6. Model Comparison and Conclusions

All models were evaluated using a common set of forecasting metrics:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- Symmetric Mean Absolute Percentage Error (sMAPE)
- Mean Absolute Scaled Error (MASE)

Among these, MASE was treated as the primary metric because it provides a scale-independent comparison relative to a naïve forecasting benchmark.

### Final Observations

- Seasonal Naïve established a strong baseline due to the pronounced annual seasonality.
- Seasonal Moving Average did not improve upon the Seasonal Naïve benchmark.
- Fourier Regression achieved the lowest forecasting errors across all evaluation metrics.
- Residual diagnostics confirmed that the model captured most of the deterministic seasonal behaviour, with only minor short-term autocorrelation remaining.
- ARIMA successfully modelled temporal dependence but was unable to match the performance of Fourier Regression.

### Lessons Learned

This project highlights the importance of understanding the underlying structure of a time series before selecting forecasting models. Rather than relying on increasingly complex models, careful exploratory analysis and statistically informed model selection produced a simpler, more interpretable solution with superior forecasting performance.

The project also reinforced several good forecasting practices:

- Preserve chronological order during model evaluation.
- Avoid information leakage by separating validation and test data.
- Establish meaningful baseline models before developing complex models.
- Use residual diagnostics to verify model adequacy rather than relying solely on forecasting metrics.
- Prefer parsimonious models when they provide comparable or superior predictive performance.

### Future Work

Potential extensions of this work include:

- Rolling-origin cross-validation for the final selected model.
- Dynamic Harmonic Regression (Fourier terms with ARIMA errors).
- Prediction intervals and probabilistic forecasting.
- Multi-station forecasting using hierarchical or panel time-series models.
- Comparison with modern forecasting approaches such as TBATS or Prophet.
