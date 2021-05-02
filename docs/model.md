# Model implementation

In this section we use the `QPM08` as an example to demonstrate the standardized procedure for implementing a new model.

---

## General steps

1. Replicate the observed data to understand how each observed variable is constructed from raw data. Store the original  files and data comparison results in `./reference/QPM08` folder.
2. Add new raw variables and observed variables according the guidance in [Data Processing](data.md) if necessary.
3. Modify the Dynare mod-file (and steady-state file), including changing the names of the observed variables and removing the estimation block. Copy the modified mod-file (and steady-state file) to `./models/QPM08`.
4. Before running estimations, make sure that the observed variables of the new model are included in the vintage data files. For more information please refer to [Data Processing](data.md).

---

## Pre-implementation

Create a folder named `QPM08` in `./reference` to store data descriptions, original data files and original mod files of the model.

Create an Excel file named `data_comparison_QPM08.xlsx` to compare the data provided by the authors with the data we construct. Minimize the discrepancy between the two as much as possible.

The only exception is for those macroeconomic series expressed in per-capita terms (such as the real GDP growth per person). For these series, we don't divide the aggregate value by the total number of population, even if the authors did so in their paper. This is because that our objective is to forecast the aggregated real GDP growth, and we do not aim at forecasting the per-capita GDP growth and population growth separately.

The graph below shows the real GDP growth calculated by the authors in blue (per capita, revised) and calculated by us in orange (aggregated, non-revised).

<figure>
  <img src="../img/CMR14GDPgrowth.PNG" width="500" />
</figure>

If the new model requires some new raw variables and/or new observed variables, please add them accordingly following the instructions in [Data Processing](data.md).

!!!note
	Before adding new variables, please always check `raw_variable_description.csv` (for raw variables) and `observed_variable_description.xlsx` (for observed variables) first, to ensure that 

!!!important
	It is essential to collect and use raw variables that are updated in high frequency whenever possible. For example, there are two federal funds rate series in the ALFRED: `FEDFUNDS` updated in a monthly frequency, and `DFF` updated in a daily frequency. Please only download and use the latter one when constructing observed variables, even if the authors used the former one in their paper.

---

## Mod-file implementation

We start from the original mod-file (and steady-state file) provided by the authors. To begin with, change the names of the observed variables in the mod-file (and steady-state file) to match the names of the variables in vintage data files.

!!!note
	Sometimes the original model does not contain the real GDP growth. Check whether it would be possible to construct a new variable (doesn't have to be the observable, but must be named `gdp_rgd_obs`) to represent the real GDP growth. For example, the graph above shows that the real GDP growth is defined in gross terms in the CMR14 model. Then we can simply define `gdp_rgd_obs = ln(original_gdp_growth)*100`. If this is not possible (for example, some models use demeaned GDP level), then try to adjust the forecast after the estimation.

Then comment out any `estimation` or `stoch_simul` block in the mod-file. We do so because a standardized estimation block will be added to the mod-file when we use `gen_forecast_old.m` to estimate models.	

Test whether the modified mod-file, in combination with vintage data files, can be correctly interpreted by Dynare for model estimation and forecast generation. If so, save the modified mod-file, steady-state file, and any relevant files (such as some functions for computing the steady state) to `./models/QPM08`.

The section [Forecast generation](forecast.md) introduces how to use implemented models to generate forecasts in multiple quarters and scenarios.

