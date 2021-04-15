# Software environments

Programs for estimating models and generating forecasts are written in **Matlab (Dynare)**.

- `Matlab 2019a + Dynare 4.5.7` are for `gen_forecast_old.m`
- `Matlab 2019a + Dynare 4.6.2` are for `gen_Forecast.m`
- `Matlab 2011a + Dynare 4.2.4` are for `gen_forecast_2011a.m`

As recommended 

---

Most programs for collecting and transforming data are wrriten in **Python 3.8.2**, a few are also written in **R (which version?)** 

The following Python libraries are used



The following 

!!! note
    It is usually .. The only exception is the `xlrd` library.

---

The online application is written in **JavaScript ES6** and is executed on **Node.js 14.6** runtime environment.

The following libraries are used

- `jQuery 3.5.1`
- `Bootstrap 4.5.0`
- `Bootstrap Table 1.16.0`
- `Chart.js 2.9.3`
- `Chart.js plugin annotation 0.5.7`

!!! note
    It is usually 

Please refer to `./application/package.json` for project dependencies.

The application is currently deployed on the free cloud service **Heroku**. For more information on how to deploy a Node.js app to Heroku please visit [this link](https://devcenter.heroku.com/articles/getting-started-with-nodejs).
