# Forecast generation

Model estimation and forecast generation are done through the Matlab m-file `gen_forecast_old.m`. Before executing this program, please make sure that

- The observed data of the model are already generated according to the descriptions in [Data processing](data.md)

- The Dynare mod-file of the model is already included in the folder `Models` according to the instructions in [Model implementation](model.md)

This program contains three blocks

- Settings
- DSGE model estimation
- Bayesian VAR model estimation

---

## Settings
All changes of the hyperparameters should be made within the section `settings`. The important parameters that needs to be checked everytime are as follows

- `p.vintages`: A string array containing the vintage dates of the model to be estimated; Date format: `yyyy-mm-dd`.
- `p.scenarios`: A string array containing the scenarios to be estimated.
- `p.models`:  A string array containing the names of the models to be estimated.

!!! note
    The names of the DSGE models should be the same as the names of the sub-folder in `Models`. The names of the BVAR models can only take three values: "GLP3v", "GLP5v", and "GLP8v".

- `p.executor`: The name of the person who runs the program.
- `p.ExcelColumnUntil`: The last column of the vintage data file. It instructs Dynare which cell range it reads from the Excel spreadsheet.
- `p.suffix`: The suffix of the names of folders that will be created in `./estimations`. It is usually used for experimental purposes if needed. A blank string is otherwise used.
- `p.chainLen`: Number of replications for the Metropolis Hastings (MH) algorithm in the estimation of DSGE models. It is set to 1,000,000 for most models, except for those with extremely high computational cost (e.g., CMR14), which is then set to 100,000.
- `p.scalingParam`: Scale parameter for the covariance matrix of the the proposal distribution in the MH algorithm. Ideally, the value of this parameter is chosen to ensure that the final acceptance rate of the MH algorithm is between 20% and 40%.
- `p.mode_compute_order`: An array containing a sequence of numbers representing the corresponding `mode_compute` algorithms in Dynare. The program loops through all the mode computation routines, until the mode is found.
 
!!!Note
    In `gen_forecast.m`, `p.mode_compute_order = 6;`. This is because an option `mh_tune_jscale` is included in the mod-file, which instructs Dynare to automatically tune the scale parameter of the covariance matrix, so that the acceptance ratio will be close to the desired level. However, this option is only compatible with `mode_compute=6`.

The usage of other hyperparameters are either self-explanatory or to be easily found in the Dynare manual.

---

## DSGE model estimation

### Model estimation

The structure of this block consists of three nested loops:

- A loop through `p.models`,
- A loop through `p.vintages`, and
- A loop through `p.scenarios`.

In each loop, a sub-folder named `[modelname]_[vintagedate]_[scenario]_(suffix)` will be created to store the estimation and forecasts under the path `./estimations`. For example, if we estimate the `QPM08` model using the vintage data at `20080807` in scenario `1` without any suffix, then a folder `QPM08_20210101_s1` will be created.

If this folder already exists, then we can choose either to delete existing files and start a complete new estimation, or keep existing files. For example, if the mode file is already in the folder, then we can keep this file to skip the maximum likelihood (ML) estimation and start the MH algorithm right away.

The vintage data and Dynare mod-file are copied to the newly created folder. Then, the estimation command is added to the bottom of the Dynare mod-file, in which we use the values of the hyperparameters given in the `settings` block.

A typical estimation command looks like the following:
>```estimation(nodisplay, smoother, order=1, prefilter=0, mode_check, bayesian_irf, datafile=data_20210209, xls_sheet=s1, xls_range=B1:AY100, presample=4, mh_replic=1000000, mh_nblocks=1, mh_jscale=0.3, mh_drop=0.3, sub_draws=5000, forecast=40, mode_compute=4) gdp_rgd_obs gdpdef_obs;```

!!!Note
    As seen from the code, it is essential to match the key observables with the following names:

    - GDP growth as `gdp_rgd_obs`, and 
    - Inflation as `gdpdef_obs`.

With the estimation command specified above, Dynare will try to obtain the ML estimate based on the fourth mode computation routine (`mode_compute=4`). If this attempt fails, the program will automatically try out the next routine contained in `p.mode_compute_order`.

### Storage of forecast

All the results are stored in the struct `oo_` after the estimation, and the program collects the nowcast and forecasts of real GDP growth (and inflation) from `oo_`.

The forecasts are stored in `t.output.forecast.gdp` for real GDP growth (and `t.output.forecast.inflation` for inflation). The first element of this array is the actual data dated one period before the current quarter. The second element is the nowcast, which are saved in different places in `oo_`:

- In Scenario one, it is calculated by Dynare and can be retreived as the first observation of the forecasted series
- In Scenario two and four, it is provided by the Survey of Professional Forecasters and can be retreived as the last observation of the smoothed series.
- In Scneario three, it is calculated dy Dynare and can be retreived as the last observation of the smoothed series.

!!!Note
	MH draws will be deleted after the estimation due to their enormous size.

Note that some forecasts have to be adjusted depending on the model of interest. 

!!!Example
	We take the first difference of the forecasts from the estimated QPM08 and IN10 models, because GDP are expressed in levels in these two models.

!!!Example
    For models in which the data are demeaned, we need to add the mean of the forecasted variables back. The IN10 model is one example. We need to run the program `IN10_adjustment_generation.m` after the estimation to add the mean of real GDP growth back.

    <!-- demeanded inflation as observable, the inflation forecast generated will also be demeaned. As a result, the MATLAB script has to be modified such that mean of the inflation has to be added back to the inflation forecast of the model. One potential way to do so is to compare the first observation (actual data) of the inflation with another model that uses non-demeaned inflation, and add the difference back to the whole forecasting series. -->

Finally, all the results are stored in a JSON file. It contains the forecasted series, model name, vintage date, scenario, and other less important information.

---

## BVAR model estimation

The program also estimates the Bayesian VAR models if at least one of the three GLP models are included in `p.models`.

The procedure is similar to the estimation of DSGE models: A new folder is created, and then data is copied to this folder. Also, after the estimation, all the results are stored in a JSON file. The only difference is that it now calls the program written by the Giannone, Lenza and Primiceri (2010) to draw samples from the posterior distribution and generate forecasts.