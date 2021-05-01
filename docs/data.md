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


### Observables registration

All descriptions and raw data that is required to construct the respective observables are registered in the excel file `observed_variable_description`, and it has to be documented manually. Some columns has to be filled before it can be constructed by the python scripts. The necessary columns are 

- Column A (id): given names of the observables
- column D (construction): raw data required for the construction of observables

!!!Note
    For column D, it is not necessary that you provide the algorithm of observables construction. If the observable is too complicated to construct, simply input all the required raw data, and seperate them with a minus `-` sign.

### Data filling

Data retrived from Alfred is easiest (as they have a series for each vintage date), but it may not contain the longest series. `Rtdsm` contains instead a longer time series, but without vintage date. As a result, we might need to prepend the same series from Rrtdsm in front of that from Alfred. The instruction of series joining is registered in 

- `fill_history.xlsx`
- `fill_nowcast.xlsx`

## Details

### Scripts
  
#### `gen_vintage_data.py`

The script generates all required observable data in an excel file with sheets named s1, s2, s3 and s4 (referring to the respective scenarios) according to specified vintage data. It acts as a function that could be called by `get_vintage_data_at_SPF_deadline.py`, which automatically retrives all the SPF deadlines.

The script first crop the relevant raw data that is need to construct the specified observables, then construct the observables according to the definition in the self defined function `construct_observables`.

If one wish to add or delete an observable, do it in the function `construct_observables`.

!!!Important
    If an extra observable is added, the generated excel sheet will have one column more. Remember to modify the parameter `p.ExcelColumnUntil` when you generate forecast!! (Please refer to [Forecast generation](forecast.md))

#### `get_vintage_data_at_SPF_deadline.py`

The script is a shell to call `gen_vintage_data.py`. The required input will be 

- the list of the id of the observables, and 
- the year of the vintage dates. 

The exact vintage is searched in the excel file `...\MMB_forecast_application\data\spf_dates.csv`

#### `raw_alfred.ipynb`

The script is a julia notebook to scrape raw data from ALFRED. To update raw data from ALFRED, simply uncomment (or add) ALFRED ID of the required series in `variables`, and ammend the `end_date` to the desire date. Raw data will be downloaded into the path `...\MMB_forecast_application\data\raw\alfred`

!!!Note
    `variables` is an array, but please note that to avoid potential errors, only download series that are with same frequency each time. A convenient way to do so would be type in series ID with same data frequency for each row, and uncomment one row each time to scrape data. 

### Data filling

#### `fill_history.xlsx` 

The excel file instructs how python prepend the data to ALFRED series in case the series in ALFRED is not long enough. The file consists of four columns

- Column A: ALFRED raw data series ID
- Column B: the source (folder) where the filling data is to be found
- Column C: Series name to fill the series in Column A
- Column D: File name of the series in Column C


#### `fill_nowcast.xlsx` 

The excel file instructs how python append the nowcast data, which is the SPF forecast. The file consists of four columns

- Column A: ALFRED raw data series ID
- Column B: the source where the filling data is to be found
- Column C: Series name to fill the series in Column A

