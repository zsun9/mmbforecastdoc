# Forecast generation

Forecast generation is done by the script `gen_forecast.m`. Beofre starting the forecast generation, please make sure that

- observables and the respective raw data is generated according to the section `Process data`
- Model mod file is added and modified according to section `Add model`

## Structure of `gen_forecast.m`

The script contains the following sections

- settings
- DSGE estimation
- Result adjustment and organization


### Settings
All changes of parameters should be made in the section `settings`. The important parameters that needs to be checked everytime are as follows

- `p.vintages`: A string vector containing the vintage dates of the model to be estimated. Date format in `yyyy-mm-dd`
- `p.scenarios`: A string vector containing the scenarios to be estimated.
- `p.models`:  A string vector containing the names of the models to be estimated.
- `p.ExcelColumnUntil`: The column of vintage data in excel file that dynare reads until. It is needed in the estimation block of dynare, as the cell rage of excel file has to be specified.
- `p.chainLen`: Number of replications for Metropolis-Hastings algorithm, it is set to 1000000 normally, except for extermely computationally costly model (such as CMR14), which is then reduced to 500000.


 Other parameters are described as follows

- `p.targetdynare`: A string vector containing the Dynare version intended to use. This parameter is only for checking purpose, and the script will produce a warning if the dynare version used is difference from the string of this parameter.
- `p.executor`: A string value that will appear in the jison file in the result. Contains the name of the excecutor of the script
- `p.suffix`: Suffix of the folder name, if needed
- `p.mode_compute_order`: A interger vector containing the sequence of `mode_compute` for Dynare to try. The next routine will be tried once the previous one runs into error.


### DSGE estimation

The algorithm of the estimation is described as follow. Looping through `p.models`, then looping through `p.vintages`, then looping throught `p.scenarios`. For each of the elements of the above vectors, a folder in the format of `XXXX_yyyymmdd_ss_fffff` is created, where `XXXX` is the name of the model, `yyyymmdd` is the vintage date, `ss` is the scenario, and `fffff` is the string value of `p.suffix`. 

For easy illustration, take the model `QPM08`, vintage date `20210101`, and senario `s1` as example. A folder `QPM08_20210101_s1_fffff` will be created under the path `...\MMB_forecast_application\estimations`. It will copy the sheet `s1` of the excel data file `data_20210101` from the path `...\MMB_forecast_application\data\vintage_data`. 

The estimation command is then built next. The estimation block will be built in the following standardized format
>estimation(nodisplay, smoother, order=1, prefilter=0, mode_check, bayesian_irf, datafile=(`data file name`), xls_sheet=(`scenario`), xls_range=B1:(`p.ExcelColumnUntil`)101, presample=4, mh_replic=(`p.chainLen`), mh_nblocks=1, mh_drop=0.3, mh_tune_jscale=0.3, sub_draws=1000, forecast=40, mode_compute=6) gdp_rgd_obs gdpdef_obs;

!!!note
    As seen from the code, it is important to match the key observables with the following names:

    - GDP growth as `gdp_rgd_obs`, and 
    - inflation as `gdpdef_obs`.

The algorithm will then append the above estimation block to the main mod file of the model that stored under `...\MMB_forecast_application\models`, and copy the appended mod file to the folder `...\MMB_forecast_application\estimations\QPM08_20210101_s1_fffff` that was created at the begining of the triple loop. The algorithm then runs the mod file using Dynare.

### Results Adjustment and Organization

Note that the MCMC draws will be deleted by the algorithm due to their enormous size. Note that some forecast results has to be adjusted depending on the model of interest (the adjustment is made under the block ` % save GDP forecasts (start from the last in-sample obs)` in the MATLAB script). 

!!!Example
    For models that uses demeanded inflation as observable, the inflation forecast generated will also be demeaned. As a result, the MATLAB script has to be modified such that mean of the inflation has to be added back to the inflation forecast of the model. One potential way to do so is to compare the first observation (actual data) of the inflation with another model that uses non-demeaned inflation, and add the difference back to the whole forecasting series.



Results will then be stored in a `.json` file in the folder. The `.json` file contains the series of forecasting values, model name, scenario, vintage date, and other informations (that are of less important).