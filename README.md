## 📌 Overview

This project predicts hourly bike rental demand using weather and temporal data. Built as my **IBM Data Analytics Capstone Project**, the system combines data collection, SQL-based exploratory analysis, predictive modeling, and an interactive R Shiny dashboard to help bike-sharing operators optimize fleet allocation.

**Key Achievement:** Achieved **79% prediction accuracy (R² = 0.79)** using Elastic Net regularization across 33 variables.

---

## 🎯 Business Problem

Bike-sharing systems face challenges with:
- Uneven bike distribution between stations
- Seasonal demand fluctuations
- Weather impact on rental behavior

**Solution:** A predictive model that forecasts hourly rental demand, enabling operators to proactively redistribute bikes and reduce shortages.

---

## 📊 Key Insights from Analysis

| Finding | Business Implication |
|---------|----------------------|
| Peak demand: 8 AM & 6 PM | Double bike availability during commute hours |
| Summer demand is 4.5× higher than winter | Reduce fleet in winter, expand in summer |
| Temperature ↑ → Demand ↑ (positive correlation) | Promote biking on warm weather days |
| Humidity ↑ → Demand ↓ (negative correlation) | Prepare for lower demand on humid days |

---

## 🛠️ Tech Stack

| Category | Technologies |
|----------|--------------|
| **Data Collection** | R, rvest (web scraping), httr (OpenWeather API) |
| **Data Wrangling** | tidyverse, stringr (regex), dplyr |
| **EDA** | SQL (RSQLite), ggplot2 |
| **Predictive Modeling** | tidymodels, glmnet (Linear/Ridge/Lasso/Elastic Net) |
| **Dashboard** | R Shiny, Leaflet, ggplot2 |

---

## 📁 Repository Structure

```
Bike-sharing-demand-prediction-system/
│
├── data/
│   ├── raw/                    # Raw datasets
│   └── processed/              # Cleaned & normalized data
│
├── notebooks/
│   ├── 01_data_collection.Rmd
│   ├── 02_data_wrangling.Rmd
│   ├── 03_sql_eda.Rmd
│   ├── 04_visualization.Rmd
│   ├── 05_baseline_model.Rmd
│   └── 06_model_refinements.Rmd
│
├── dashboard/
│   ├── ui.R                    # Shiny UI
│   ├── server.R                # Shiny server
│   └── model_prediction.R      # Prediction logic
│
├── data/
│   ├── selected_cities.csv
│   └── model.csv               # Trained coefficients
│
├── README.md
└── requirements.txt
```

---

## 🔧 Installation & Setup

### Prerequisites
- R (version 4.0+)
- RStudio (recommended)

### Install Required Packages

```r
install.packages(c(
  "tidyverse", "tidymodels", "RSQLite", "shiny", 
  "leaflet", "ggplot2", "httr", "rvest", "glmnet"
))
```
### Set Up API Key

1. Sign up for a free API key at [OpenWeatherMap](https://home.openweathermap.org/users/sign_up)
2. Replace the API key in `model_prediction.R`:

```r
api_key <- "your_api_key_here"
```

## 📈 Model Performance

### Models Compared

| Model | R-squared | RMSE |
|-------|-----------|------|
| Baseline Linear | 0.750 | 0.090 |
| Polynomial (deg 2) | 0.755 | 0.089 |
| Polynomial (deg 3) + Interactions | 0.770 | 0.086 |
| Ridge Regression | 0.765 | 0.087 |
| Lasso Regression | 0.765 | 0.087 |
| **Elastic Net (Best)** ✅ | **0.790** | **0.083** |

### Best Model Formula

```r
RENTED_BIKE_COUNT ~ . + 
  poly(TEMPERATURE, 3, raw = TRUE) + 
  poly(HUMIDITY, 3, raw = TRUE) + 
  RAINFALL:HUMIDITY + 
  TEMPERATURE:HUMIDITY
```

### Top Predictors (by coefficient magnitude)

| Variable | Coefficient | Impact |
|----------|------------|--------|
| HOUR_18 (6 PM) | +661 | Strong positive |
| HOUR_8 (8 AM) | +419 | Strong positive |
| TEMPERATURE | +25 | Positive |
| HUMIDITY | -8.45 | Negative |

---

## 🖥️ Dashboard Features

### Interactive Map (All Cities View)
- Circle markers sized by predicted demand
- Color-coded: 🟢 Small (<1000) | 🟡 Medium (1000-3000) | 🔴 Large (>3000)
- Popup shows city name, prediction, and demand level

### Single City View
- **Temperature trend line** (5-day forecast)
- **Bike demand trend line** (click points for details)
- **Humidity vs demand scatter plot** (with polynomial smooth line)

---

## 📋 EDA SQL Queries Performed

| Query | Result |
|-------|--------|
| Record count | 8,465 hourly records |
| All-time high | 3,556 rentals (June 19, 2018, 6 PM) |
| Best season-hour | Summer, 6 PM (2,135 avg rentals) |
| Seasonal demand | Summer 4.5× higher than Winter |
| Seoul total bikes | 20,000 bikes |
| Comparable cities | 5 Chinese cities (15,000-20,000 bikes) |

---

## 🏆 Key Achievements

✅ **79% prediction accuracy** (R² = 0.79) exceeding project requirement of 0.72

✅ **RMSE reduced by 7.8%** from baseline through polynomial terms, interactions, and regularization

✅ **Interactive dashboard** with Leaflet mapping and real-time API integration

✅ **Full data pipeline** from collection (web scraping + API) to deployment

---

## 📚 Lessons Learned

1. **Weather matters** – Temperature and humidity are the strongest environmental predictors
2. **Time is everything** – Rush hours (8 AM, 6 PM) dominate demand regardless of season
3. **Regularization helps** – Elastic Net balanced bias-variance tradeoff best
4. **API integration is powerful** – Real-time weather data enables live predictions

---

## 🔮 Future Improvements

- [ ] Add more cities to the dashboard
- [ ] Incorporate day-of-week and holiday effects
- [ ] Deploy Shiny app to shinyapps.io or Docker
- [ ] Add bike station-level predictions (not just city-level)
- [ ] Integrate live traffic data for better commute predictions

---

## 📜 Certificate

This project was completed as part of the **[IBM Data Analytics with Excel and R Professional Certificate](https://www.coursera.org/professional-certificates/ibm-data-analyst)**.

---
