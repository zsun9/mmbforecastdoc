# Model Implementation

In this model implementation section, the model "QPM08" is used as an example. Implemetation of another moel is done by simply replacing "QPM08" with the desired name of the model.

## General Steps
Model is implemented in the following strucuture

- Replicate the data and understand the algorithm in constructing the observables
- Observations has to be constructed in the Python script `MMB_forecast_application\scripts\get_vintage_data.py`
- The mod file is stored in the folder `MMB_forecast_application/models/QPM08`
- the estimation block of the mod file has to be commented out, as the main matlab script will add the comment block automatically
- Data of the specific vintage date will be retrived from the folder `MMB_forecast_application\data\vintage_data`

## Pre-implementation
First observable data of the model has to be replicated. To download required raw data, please check section `Process data`. Once we are able to replicate the data provided by the author, we could implement the data in the Python script `MMB_forecast_application\scripts\get_vintage_data.py`.

## Mod File Implementation

We start from the original mod file provided by the author. If there is a steady-state file provided by the author, also include it in the model folder. For the mod file, We take special care of the block `varobs`, as it contains the observational variables that we have constructed in vintage data. For the mod file, names of the variables appeared in `varobs` has to be modified to match the names of variables generated in vintage data. Note that one could use the function "Find and Replace All" of any editors.

We then comment out any estimation or stoch_simul block in the mod file. We do so becasue the unified estimation block will be added to the mod file by the MATLAB sciprt `gen_forecast.m`. Both the mod file and its steady-state file (if available) is to be stored in `MMB_forecast_application/models/QPM08`.


## Reference
The original code and pdf of the paper is to be stored under the folder `MMB_forecast_application\reference\QPM08`. 

## Estimation and Generating Forecast
Please see section `Generate forecast`

