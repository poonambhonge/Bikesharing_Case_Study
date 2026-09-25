# Bike Sharing Demand Prediction

A multiple linear regression model that predicts daily demand for shared bikes and identifies the factors that drive it, helping a US bike-sharing company (BoomBikes) plan for recovery after the COVID-19 lockdown.

## Business Problem

BoomBikes saw a sharp drop in revenue during the pandemic and wants a business plan to accelerate growth once the lockdown ends. The company wants to know:

1. **Which variables are significant** in predicting demand for shared bikes.
2. **How well those variables explain** bike demand.

## Dataset

`day.csv`: **730 daily records × 16 columns** covering 2018–2019, with season, year, month, holiday, weekday, working day, weather situation, temperature, humidity, wind speed, and total rentals (`cnt`, the target).

## Approach

1. **Data understanding and cleaning:** checked for nulls and outliers (none found); dropped `instant` (row ID), `dteday` (date features already exist), and `casual` / `registered` (they add up to `cnt`, so keeping them would leak the target).
2. **EDA:** pair plots, correlation heatmaps, and box plots of demand against season, year, month, holiday, weekday, and weather. `temp` and `atemp` were over 0.99 correlated, so only one was kept.
3. **Data preparation:** converted season, month, weekday, and weather codes into labelled categories and created dummy variables; 70:30 train-test split; Min-Max scaling of continuous variables.
4. **Model building:** Recursive Feature Elimination (RFE) for initial feature selection, then manual backward elimination using p-values (< 0.05) and VIF (< 5) across **7 models** with `statsmodels` OLS.
5. **Validation:** residual analysis (normality and randomness of error terms), then predictions and R² on the test set.

## Results

| Metric | Value |
|---|---|
| Train R² | **0.818** |
| Adjusted R² | **0.815** |
| Test R² | **0.820** |
| Max VIF | < 5 (no multicollinearity) |
| p-values | All < 0.05 |

The test R² almost matches the train R², so the model **generalises well to unseen data**.

### Final Model: Significant Variables

| Variable | Coefficient | Effect on demand |
|---|---|---|
| `temp` | +3423.28 | Higher temperature → strongly higher demand |
| `yr` | +2000.33 | Demand grew year on year (2019 vs 2018) |
| `mnth_sept` | +546.30 | September is a peak month |
| `season_winter` | +429.55 | Winter adds demand compared with fall/summer when temperature is the same |
| `weathersit_moderate` | −647.33 | Mist / cloudy weather reduces demand |
| `holiday` | −708.56 | Lower demand on holidays |
| `windspeed` | −747.87 | Higher wind speed reduces demand |
| `season_spring` | −1246.70 | Spring has the lowest demand |
| `weathersit_bad` | −2212.15 | Light rain / snow sharply reduces demand |

**Intercept:** 2375.84

## Key Insights & Recommendations

- **Temperature is the strongest driver:** plan fleet size and promotions around warmer periods.
- **Demand is growing year on year,** so expect a strong rebound once normal conditions return.
- **Fall and September are peak times:** expand capacity and marketing then.
- **Spring is the weakest season:** use discounts or campaigns to lift demand.
- **Bad weather, high wind, and holidays cut demand:** schedule maintenance and rebalancing on these days.

## Tools & Libraries

Python · Pandas · NumPy · Matplotlib · Seaborn · scikit-learn (RFE, LinearRegression, MinMaxScaler) · Statsmodels (OLS, VIF) · Jupyter Notebook

## Repository Contents

| File | Description |
|---|---|
| `Poonam Bhonge== Bike sharing Assignment.ipynb` | Full notebook: EDA, model building, and evaluation |
| `day.csv` | Dataset |
| `Linear Regression Subjective Questions.pdf` | Answers to the assignment's subjective questions |

---

*Completed as part of the Executive PG Programme in Data Science, IIIT Bangalore.*
