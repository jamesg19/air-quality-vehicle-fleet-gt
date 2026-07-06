# Light Vehicle Fleet Growth and PM2.5 Air Quality in Guatemala City (2022–2025)

Time-series study of whether the sustained growth of Guatemala City's light
vehicle fleet explains the deterioration in air quality measured by PM2.5.
The analysis follows the **CRISP-DM** framework and is the empirical core of a
Master's thesis in Data Science (USAC, Faculty of Engineering).

## Key finding

At a monthly scale, the light vehicle fleet volume is **not** a statistically
significant predictor of PM2.5 once temporal structure and seasonality are
controlled for. The main contribution is methodological: it demonstrates that
direct level correlations are **spurious** (the series have different
integration orders) and that **seasonal/meteorological factors dominate** over
vehicle volume in explaining monthly PM2.5 variability.

## Data

| Source | Description | Records |
|--------|-------------|---------|
| SAT (Superintendencia de Administración Tributaria) | Light vehicle registrations (altas) | 499,743 |
| PM2.5 daily monitoring series | Daily fine particulate concentrations | 1,380 |

Effective study window after cleaning: **46 monthly observations** (Mar 2022 – Dec 2025).
One physically implausible PM2.5 outlier (1,016 µg/m³) was removed.

## Methodology (CRISP-DM)

1. **Business/research understanding** — hypothesis on fleet vs. PM2.5.
2. **Data understanding** — exploration of both datasets.
3. **Data preparation** — monthly aggregation, cumulative circulating stock, consolidated dataset.
4. **Modeling** — ADF stationarity tests, ARIMA baseline, SARIMAX with exogenous variable.
5. **Evaluation** — AIC/BIC/RMSE, residual diagnostics (Ljung-Box, Shapiro-Wilk, Jarque-Bera).
6. **Advanced analysis** — Engle-Granger cointegration, differenced-series correlation, OLS by vehicle type.

## Results at a glance

| Test / Model | Result | Interpretation |
|--------------|--------|----------------|
| ADF — PM2.5 | p = 0.0106 | Stationary, I(0) |
| ADF — Cumulative fleet | p = 1.0000 | Non-stationary, I(1) |
| SARIMAX exogenous coef. | p = 0.589 | Vehicle effect not significant |
| ARIMA vs SARIMAX (AIC) | 387.36 → 381.09 | SARIMAX marginally better fit |
| Engle-Granger cointegration | p = 0.0319 | Long-run relationship detected |
| OLS by vehicle type | R² = 0.157 | Weak explanatory power |

## Repository contents

- `air_quality.ipynb` — full CRISP-DM notebook (80 cells).
- `Data.zip` — source datasets (SAT vehicle registry and PM2.5 daily series).
- `requirements.txt` — Python dependencies.

## Requirements

```bash
pip install -r requirements.txt
```

Tested with Python 3.11+.

## How to run

```bash
git clone https://github.com/jamesg19/pm25-vehicle-fleet-guatemala.git
cd pm25-vehicle-fleet-guatemala
unzip Data.zip
jupyter notebook air_quality.ipynb
```

> **Data:** The source datasets are provided in `Data.zip`. Unzip it into the
> project root (or the path referenced in the notebook) before running the
> notebook to reproduce the results.

## Limitations

This analysis does not establish causality. Climate controls (temperature,
precipitation) and industrial/agricultural burning are not yet included; the
evidence suggests these dominate. A natural extension is a multivariate model
(SARIMAX or ECM) incorporating climate regressors.

## Author

**James Gramajo** — Systems and Sciences Engineer.
Master's student in Engineering for Industry (Data Science),
School of Postgraduate Studies,
Faculty of Engineering, Universidad de San Carlos de Guatemala (USAC).

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.