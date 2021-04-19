# Data processing

## Overview

### Raw data storage
Raw data is stored in the path `...\MMB_forecast_application\data\raw`. Raw data is data that downloads directly from source without being processed. The current source are from Alfred, Greenbook, Rtdsm, Spf and "others". The observable data generated from raw data is stored under the path `...\MMB_forecast_application\data\vintage_data`.

### Scripts overview

Data retriving and generating scripts are stored in the path `...\MMB_forecast_application\scripts` The following scripts are important in generating the desired observable data:

- `gen_vintage_data.py`: retrives data from raw data and compute the observational variables
- `get_vintage_data_at_SPF_deadline.py`: Outer shell to call `gen_vintage_data.py` as a function to generate excel sheets in folder `vintage_data`
- `raw_alfred.ipynb`: Script to retrieve raw data according to vintage dates in Alfred

### Data discriptions

Raw data and computed observables has to be documented in the following excel files before they can be retrived by the Python scripts. It serves as a checking, and will run into error if missed. The data discription files is stored under the path `...\MMB_forecast_application\data`

- `raw_variable_description.csv`: Stores the characteristics of raw data. For raw data that is downloaded from Alfred, the documentation of description is automatically done. Otherwise, manaul documentation is needed. 
- `observed_variable_description`: Stores the discriptions of observables, and has to be documented manually.

### Data filling

Data retrived from Alfred is easiest (as they have a series for each vintage date), but it may not contain the longest series. Rtdsm contains instead a longer time series, but without vintage date. As a result, we might need to prepend the same series from Rrtdsm in front of that from Alfred. The instruction of series joining is registered in 

- `fill_history.xlsx`
- `fill_nowcast.xlsx`

## Details
### Scripts 
#### `gen_vintage_data.py`

xxxxxxxxxxxxxxx

#### `get_vintage_data_at_SPF_deadline.py`

xxxxxxxxxxxxxxxxx

#### `raw_alfred.ipynb`

xxxxxxxxxxxxxxxxxxx

