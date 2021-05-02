# Online application

The forecasts, RMSEs as well as the time-series plots of the observables are all presented in the online application.

The application is accessible locally through [Visual Studio Code](https://code.visualstudio.com)'s `Live Server` extension.

!!!note
	As the application is built with [Node.js](https://nodejs.org/en/), one can also start the application locally by running the command `node app.js`.

---

## Results collection

Forecasts from DSGE and BVAR models are stored in JSON format in each sub-folder of `./estimations`. Forecasts from the SPF, Greenbook, and Fair's Cowles Commission type model are stored in the Excel file `./data/gb_spf_fair.xlsx`.

Before showing these forecasts in the application, one needs to run `./scripts/collect_results.py` once to

- collect model-based forecasts from JSON files
- collect forecasts from SPF, Greenbook, and Fair
- collect actual historical values of the GDP growth
- calculate the RMSEs of the forecasts
- calculate mean forecasts for a collection of pre-crisis models and a collection of post-crisis models
- save the above results to `./application/src/results.json`
- collect observed data in `./data/vintage_data` and save them to `./application/src/variables.json`.

Data in `results.json` and `variables.json` are then fetched by the application to present forecasts, RMSEs and time-series plots of the observed variables.

!!! note 
	After implementing a new model, please specify whether it is a pre-crisis or post-crisis model in `./data/ModelClass.xlsx`, so that it can be correctly identified in `collect_results.py` when calculating mean forecasts of pre- and post-crisis models.
 
<!--  In the dictionary there are five keys

- `actual`: contains the actual data
- `dsge`: contains the forecasting estimation of different dsge models with different vintage dates, also the mean forecast of the models with the name `Post-crisis models avg` and `Pre-crisis models avg` 
- `error`: The forecasting error for the RMSE table
- `external`: external forecasts, such as SPF mean and SPF individual
- `ts`: contains the forecasting estimation of different time-series models with different vintage dates  -->

---

## Styles

The styles of the HTML pages are defined in `./application/css/stylesheet.css`, which specifies font types, font sizes, margins of the HTML elements and Bootstrap components, etc.

Colors and styles of the lines in the forecast charts are specified in `./application/js/constants.js`.

---

## Code structure

HTML pages are written within the [Bootstrap](https://getbootstrap.com) framework. Common components, such as the footer and navigation bar, are written in `./application/common` and are shared across different webpages. 

- `index.html`: The index page shown to visitors.
- `login.html`: The login page.
- `index_login.html`: The index page shown to logged-in users.
- `forecast.html`: The page for visualizing the forecasts.
- `rmse.html`: The page for displaying RMSEs.
- `variable.html`: The page for showing time-series plots of the observables.
- `about.html`: The page that provides basic information about the application
- `app.js`: Scripts for building up the `Node.js` application.

### `forecast.html`

The script block of the page does the following

1. Load common components to the page
2. Fetch information about models, vintage quarters, and scenarios from `results.json` to build up checkboxes for users to select
3. When users finish the selection and click the submit button
	- retrieve the selection
	- collect the corresponding forecasts and actual data
	- draw forecast charts using the `Chart.js` library

### `rmse.html`

The script block of this page performs similar functions as the previous script. The difference is that, after retrieving user's selection, it collects the RMSEs from `results.json` and create tables to present the results using the `Bootstrap Table` library.

### `variable.html`

The script block of this page performs similar functions as the previous script. The difference is that, after retrieving user's selection, it collects the historical values of the observed variables from `variables.json` and draw time-series plots using the `Chart.js` library.

