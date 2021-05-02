# Data processing

Both raw data and observed data are stored in the `data` folder. Most raw data can be automatically downloaded from web and transformed into observed data that are used for model estimation.

The remainder of the page introduces [where to store data](#data-storage), [how to generate observed vintage data from raw data](#generate-vintage-data), [how to update raw data](#update-raw-data), [how to add new data](#add-new-data), and [some further information](#further-information).

---

## Data storage

The path `./data/raw` contains raw data that were directly downloaded from the source without being processed. There are five data sources:

- `alfred`: raw data from the [Archival FRED](https://alfred.stlouisfed.org), which contains vintage versions of economic data from various original sources, including the Bureau of Economic Analysis, the Bureau of Labor Statistics, and the Federal Reserve Board.

- `greenbook`: forecasts from [the Federal Reserve Board's Greenbook](https://www.philadelphiafed.org/surveys-and-data/real-time-data-research/greenbook) (with a five-year lag)

- `spf`: forecasts from [the Survey of Professional Forcasters (SPF)](https://www.philadelphiafed.org/surveys-and-data/real-time-data-research/survey-of-professional-forecasters)

- `rtdsm`: raw data from the Philadelphia Fed's [Real Time Data Set for Macroeconomists](https://www.philadelphiafed.org/surveys-and-data/real-time-data-research/real-time-data-set-for-macroeconomists)

!!!Note
	We use raw data from the ALFRED to construct observed data. We compare the model-based forecasts with the forecasts from the SPF or Greenbook. Raw data from the RTDSM are usually not used.

- `others`: raw data collected from other sources.

The path `./data/vintage_data` contains observed vintage data for model estimation. Each Excel spreadsheet contains observed data that are collected in a specific vintage date for four scenarios.

---

## Generate vintage data

The main program for generating observed vintage data is `./scripts/gen_vintage_data.py`. It first collects relevant raw data that are needed for constructing the specified observed variables, and then calculates observed variables according to the definitions in the block `construct_observables`.

As an example, by executing the following command, Python will generate a new Excel file in `./data` that contains three observed variables: `gdp_rgd_obs`, `ffr_obs` and `gdpdef_obs`. The sample period is from 1990:I to 2008:III, and the data was observed in Aug 7th, 2008.

>gen_vintage_data(vintageDate='2008-08-07', quarterStart='1990Q1', quarterEnd='2008Q3', raw=[], observed=['gdp_rgd_obs', 'ffr_obs', 'gdpdef_obs'])

The program `./scripts/get_vintage_data_at_SPF_deadline.py` repeatedly calls `gen_vintage_data.py` to generate a bunch of Excel files. Each Excel file contains data observed at a specific SPF submission deadline. The required inputs are

- A list of the IDs of the observed variables, and 
- The years (and months) when the corresponding SPFs are made

We need only fill in the years (and months) when the SPFs are done, as Python searches for the exact SPF submission dates in `./data/spf_dates.csv`.

!!!Note
	Please remember to include new SPF deadlines in `spf_dates.csv` based on the updated information on [this page](https://www.philadelphiafed.org/-/media/frbp/assets/surveys-and-data/survey-of-professional-forecasters/spf-release-dates.txt).

!!!Note
	Newly generated vintage data files will appear in `./data`. Please move these files to `./data/vintage_data` (and replace the old files when necessary) if you find no mistakes in them.

!!!Important
    If more observed variables are included in the vintage data file, please adjust the hyper-parameter `p.ExcelColumnUntil` in the Matlab program accordingly. More information can be found in [Forecast generation](forecast.md))

---

## Update raw data

`./scripts/raw_alfred.ipynb` is a Python Jupyter notebook for collecting raw data from the ALFRED. To update raw data, simply uncomment (or add) the ID of the required series in the first array `variables`, and amend the `end_date` to the desire date. Raw data will then be downloaded to the path `./data/raw/alfred`.

!!!Note
    Ideally, `variables` would contain the IDs of all the data series. However, due to some unknown issues regarding ALFRED's API, one can only download series that have same frequency each time.  A convenient way to do so is to type in IDs of the series that share the same frequency for each row, and uncomment one row each time to download data. 

!!!Note
	When raw data are being downloaded, their basic information are also being automatically written to `./data/raw_variable_description.csv`. Raw data has to be documented in this file before they can be retrieved by the Python scripts.

`./scripts/raw_rtdsm_gb_spf.py` is a Python program for scraping raw data from the RTDSM, Greenbook, and SPF. Data from these sources will be downloaded to the paths `./data/raw/rtdsm`, `./data/raw/greenbook`, and `./data/raw/spf`, respectively.

One has to manually update all the data series in `./data/raw/others`. More details can be found in `./data/raw_others_collection.txt`.

---

## Add new data

### Add new raw variable

- If the new raw variable is from the ALFRED, then one can simply include its ID in the first array of the Notebook `./scripts/raw_alfred.ipynb`.

- If the new raw variable is from elsewhere, one needs to manually collect and store its values in an Excel spreadsheet in `./data/others`. The name of the spreadsheet should be identical to the ID of this variable.

	- If the historical values of this variable are not revised over time (e.g., asset prices from the financial market), then we just need to record its values in one column. Check `C0091Y.xlsx` as an example.

	- If the historical values of this variable are revised (e.g., most macroeconomic series) and it is possible to collect its historical values in each revision, then it is recommended to record its values in a upper-triangular matrix. Check `MORTRATE.xlsx` as an example.

!!!Note
	For raw variables that are not from ALFRED, one needs to manually document their basic information in `raw_variable_description.csv`.


### Add new observed variable

To begin with, a new observed variables has to be manually registered in a new row in the Excel file `./data/observed_variable_description`. Two columns has to be filled before it can be constructed by the Python program:

- Column A (id): name of the new observed variable, which should be different from the existing names already listed in this file
- column D (construction): IDs of the raw data that are required for constructing this new observable

!!!Note
    For column D, one can simply list the IDs of all the required raw variables and separate them with a minus `-` sign.

After the registration, one needs to write down the formula for constructing the new observed variable in the block `construct_observables` of the program `./scripts/get_vintage_data.py`.

The basic syntax is as follows

``` python
elif obs == [ID_of_observable]:
	df.loc[:, obs] = [construction formula]
```

Use the syntax `df[d[ID_of_raw_variable]]` to incoporate raw variables in the formula.

For example, the following scripts create a new observable `Y`, which is defined as the log difference of the raw variable `X` in percentage terms

``` python
elif obs == 'Y':
    df.loc[:, obs] = np.log(df[d['X']]/df[d['X']].shift())*100
```

---

## Further information

### Determine the type of DSGE model

Information contained in the Excel file `./data/ModelClass.xlsx` determines a DSGE model belongs to the pre-crisis or post-crisis type. This is useful when we run `collect_results.py` to calculate the mean forecasts of pre- and post-crisis models afterwards.

### Actual data to be compared with

We compare the forecasts of the real GDP growth with its actual values in the second release. These actual values are saved in `./data/actualGDP.csv`. Please remember to update this file if new data become available.

### Forecasts to be compared with

We compared the model-based forecasts with forecasts from other sources, which are saved `./data/gb_spf_fair.xlsx`. It contains the forecasts of the real GDP growth from

- Greenbook (wtih a five-year lag)
- SPF (including both individual forecasts and mean forecasts), and
- Fair's Cowles Commission type model

Please remember to update this file if new forecasts become available.

### Filling missing values

We choose to use raw data from the ALFRED rather than RTDSM to construct observed data, because the former database usually contains richer information about data revisions in history. However, sometimes there are some missing values in the data files downloaded from ALFRED, and then we need to use the same series observed on the same vintage date from the RTDSM to fill these values.

When generating vintage data, Python leverages the information in the Excel file `./data/fill_history.xlsx` to check if it is possible to fill some missing values.

- Column A: ID of the raw series from the ALFRED
- Column B: The source (folder) where the filling data is to be found
- Column C: ID of the raw series in RTDSM for filling missing values
- Column D: File name of the series in Column C

### Filling nowcast

!!!warning
	Proceed with caution. It is advisable to consult senior members of the team before filling the nowcast of some new variables.

In Scenarios 2 and 4 we append the mean of the SPF nowcast as additional observations in the last sample period. The Excel file `./data/fill_nowcast.xlsx` instructs Python how to append the data.

- Column A: D of the raw series from the ALFRED
- Column B: The source where the filling data is to be found (usually SPF)
- Column C: ID of the raw series for filling nowcast
