# Predictive Maintenance — Industrial Pump Sensor Analysis

End-to-end time series analysis on industrial IoT sensor data to identify early-warning signals preceding equipment failure, supporting a predictive maintenance use case.

---

## Dataset
- **Source:** Industrial pump sensor readings
- **Period:** April 2018 – August 2018
- **Size:** 220,320 rows × 52 sensors (1-minute resolution)
- **Target:** `machine_status` — NORMAL / RECOVERING / BROKEN

---

## Analysis Pipeline

| Step | Description |
|---|---|
| Missing Value Treatment | Forward-fill + back-fill (limit=30 min); dropped sensor_15 (100% null) |
| Sensor Selection | Variance-ranked top-6 sensors from 52-channel dataset |
| Seasonal Decomposition | Trend / Seasonal / Residual split (24h period) on sensor_00 |
| Stationarity Testing | Augmented Dickey-Fuller (ADF) test on top-6 sensors |
| Autocorrelation Analysis | ACF & PACF plots up to 72-hour lag |
| Rolling Statistics | 24h rolling mean ± 2σ band |
| Distribution Analysis | KDE plots by machine status (NORMAL / RECOVERING / BROKEN) |
| Correlation Analysis | Pearson heatmap across top-20 sensors |
| Dimensionality Reduction | PCA — 52 sensors reduced to 14–15 independent dimensions |
| Anomaly Detection | Isolation Forest (2% contamination) on hourly sensor data |

---

## Key Findings

- **Class imbalance:** NORMAL 93.4%, RECOVERING 6.6%, BROKEN 0.003% (7 events only)
- **Distributional shift:** Key sensors show visibly different value distributions between healthy and degraded machine states
- **Sensor redundancy:** Two high-correlation clusters identified (r > 0.98); 52 sensors collapse to ~15 independent dimensions via PCA
- **Anomaly detection:** Isolation Forest flags RECOVERING hours at 1.7× the rate of NORMAL hours, validating unsupervised pre-failure alerting

---

## Tech Stack

```
Python · pandas · NumPy · statsmodels · scikit-learn · matplotlib · seaborn
```

---

## Files

```
├── sensor_timeseries_analysis.ipynb   # Full executed notebook with outputs
├── README.md
```

---

## How to Run

```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn
jupyter notebook sensor_timeseries_analysis.ipynb
```
