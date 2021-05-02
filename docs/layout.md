# Project layout

This page briefly introduces the files stored in the repository.

---

## Files in the root folder

- `gen_forecast_old.m`: main program for model estimation and forecast generation

- `gen_forecast.m`: new program for model estimation, in which the scale factor of the MH algorithm are automatically tuned based on a new option added in Dynare 4.6.2.

!!! warning
    There are some discrepancies between the forecasts obtained from `gen_forecast_old.m` and from `gen_forecast.m`, which cannot be fully explained by the exclusion / inclusion of the auto-tuning process. To keep the results consistent, we still use `gen_forecast_old.m` to calculate the forecasts.

- `gen_forecast_2011a.m`: program for estimating the KR15_FF and KR15_HH models only, which should be executed in Matlab 2011a with Dynare 4.2.4.

- `mat2json.m`: function for converting Matlab structures to JavaScript data objects (JSON).

- `IN10_adjustment_generation.m`: program for adjusting the forecasts based on the estimated IN10 model. It adds the steady-state real GDP growth rate back.

---

## Application

This folder contains files for building up the online application. Details of these files can be found in [Online application](application.md).

---

## Archived

This folder contains files that are no longer in use.

---

## Data

This folder contains the raw data, vintage data, and data description files. Details of these files can be found in [Data processing](processing.md).

---

## Estimations

This folder contains the estimation and forecasting results. More information can be found in [Forecast generation](forecast.md).

---

## Models

This folder contains Dynare mod-files for model estimation. More information can be found in [Model implementation](model.md)

---

## Outputs

This folder contains files that document the main results of the project.

---

## Reference

This folder contains the original papers, technical appendix, and data files provided by the authors, as well as data comparison results.

---

## Scripts

This folder contains programs for processing data and updating results.