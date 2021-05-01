# Project layout

This page briefly introduces the files stored in different folders of the GitHub repository.

---

## Files in the root folder

- `gen_forecast_old.m`: main program for estimating models and generating forecast.

- `gen_forecast.m`: program estimating models, in which the scale factor of the MH algorithm are automatically tuned based on a new option added in Dynare 4.6.2.

!!! warning
    There are some discrepancies between the forecasts obtained from `gen_forecast_old.m` and from `gen_forecast.m`, which cannot be fully explained by the exclusion / inclusion of the auto-tuning process. To keep the results consistent, we still use `gen_forecast_old.m` to calculate the forecasts.

- `gen_forecast_2011a.m`: program for estimating the KR15_FF and KR15_HH models only, which should be executed in Matlab 2011a with Dynare 4.2.4.

- `mat2json.m`: program for converting Matlab structures to JavaScript data objects (JSON)

- `IN10_adjustment_generation.m`: program for adjusting the forecasts based on the Iacoviello and Neri (2010) model. The steady-state growth rate is set to zero in the model, and this program add this growth rate back.

---

## Application

This folder contains files for building up the online application.

- `index.html`: The index page.
- `login.html`: The login page.
- `index_login.html`: The index page after a user is logged in.
- `forecast.html`: The page for visualizing the forecasts.
- `rmse.html`: The page for displaying root mean squared errors (RMSEs).
- `variable.html`: The page for showing time-series plots of the observables.
- `about.html`: The page that provides basic information about the application
- `app.js`: 
- `package.json`:
- `common`: Contains HTML components shared across webpages.
- `css`:
- `fonts`:
- `js`:
- `node_modules`: Contains 
- `src`:

---

## Archived

This folder contains files that are no longer in use.

---

## Data

This folder contains the raw data, vintage data, and data description files.

---

## Estimations

This folder contains the estimation results.

---

## Models

This folder contains Dynare mod-files for model estimation.

---

## Outputs

This folder contains spreadsheets and documents that display the main results of the project.

---

## Reference

This folder contains the original papers, technical appendix, and data files provided by the authors, as well as data comparison results.

---

## Scripts

This folder contains programs for processing data and updating results.