# Alinta Energy Demand Prediction

This repository contains the analysis used for an RMIT Individual Task 1 case study linked to the Data Scientist role at Alinta Energy.

## Professional problem

Can historical electricity demand and weather conditions be used to predict Victorian electricity demand?

## Data sources

### AEMO Operational Demand
Australian Energy Market Operator operational-demand data were used for the `VIC1` region.

Source:
https://www.aemo.com.au/energy-systems/electricity/national-electricity-market-nem/data-nem/operational-demand-data

Study period:
1 September 2025 to 31 July 2026.

The downloaded five-minute data were aggregated to hourly average operational demand in MW.

### Open-Meteo Historical Weather
Hourly historical Melbourne weather data were obtained from Open-Meteo.

Source:
https://open-meteo.com/en/docs/historical-weather-api

Variables used:
- temperature at 2 metres
- relative humidity
- precipitation
- wind speed at 10 metres

## Analysis workflow

The notebook:

1. reads the AEMO Operational Demand ZIP file;
2. filters the data to `VIC1`;
3. aggregates electricity demand to hourly resolution;
4. retrieves matching Melbourne historical weather;
5. aligns timestamps to AEST/NEM market time;
6. merges demand and weather by timestamp;
7. creates calendar and historical-demand features;
8. uses a chronological 80/20 train-test split;
9. trains a Decision Tree Regressor and Random Forest Regressor;
10. compares both models with a same-hour-previous-day baseline;
11. evaluates performance using MAE, RMSE, R² and peak MAE;
12. tests whether adding weather variables improves Random Forest performance.

## Main results

On the chronological test set:

- Random Forest MAE: 206.49 MW
- Random Forest RMSE: 294.55 MW
- Random Forest R²: 0.9122
- Random Forest peak MAE: 255.98 MW
- MAE improvement over the previous-day baseline: 51.55%
- Adding weather reduced Random Forest RMSE by 4.70%

The strongest Random Forest predictive features were one-hour lagged demand, hour-of-day and temperature.

## Reproducing the analysis

Open:

`Alinta_Energy_Demand_Analysis_Colab.ipynb`

in Google Colab.

Download the latest AEMO five-minute Actual Operational Demand ZIP from:

https://nemweb.com.au/Reports/Current/Operational_Demand/ACTUAL_5MIN/

Run the notebook from top to bottom and upload the AEMO ZIP when prompted.

Open-Meteo weather data are retrieved automatically by the notebook.

## Notes

The raw AEMO ZIP is not stored in this repository because it is publicly available from AEMO and changes as the rolling dataset is updated.

AEMO notes that its five-minute operational-demand series is interpolated from half-hourly measurements. The analysis therefore aggregates demand to hourly resolution and treats the work as a prototype rather than a production electricity-demand forecast.
