# Software environments

## Matlab (Dynare)

Programs for estimating models and generating forecasts are written in `Matlab (Dynare)`.

- `Matlab 2019a + Dynare 4.5.7` are for `gen_forecast_old.m`
- `Matlab 2019a + Dynare 4.6.2` are for `gen_Forecast.m`
- `Matlab 2011a + Dynare 4.2.4` are for `gen_forecast_2011a.m`

As recommended 

## Python and R

Most programs for collecting and transforming data are wrriten in `Python 3.7.0`, a few are also written in `R 4.0.2`.

The following Python libraries are used

- `xlrd 1.2.0`
- `pandas 1.0.1`
- `numpy 1.18.1`
- `simplejson 3.17.2`
- `tqdm 4.42.1`

The following R libraries are used

- `stringr 1.4.0`
- `readxl 1.3.1`

!!! note
    There is generally no need to install a specific version of any Python or R modules listed above. The only exception is the `xlrd` module: Please make sure that you have version `1.2.0` installed.

## JavaScript

The online application is written in `JavaScript ES6` and is executed on `Node.js 14.6` runtime environment.

The following libraries are used

- `jQuery 3.5.1`
- `Bootstrap 4.5.0`
- `Bootstrap Table 1.16.0`
- `Chart.js 2.9.3`
- `Chart.js plugin annotation 0.5.7`

!!! note
    There is generally no need to use a specific version of any JavaScript modules listed above.

Please refer to `./application/package.json` for project dependencies.

The application is currently deployed on the free cloud service **Heroku**. For more information on how to deploy a Node.js app to Heroku please visit [this link](https://devcenter.heroku.com/articles/getting-started-with-nodejs).
