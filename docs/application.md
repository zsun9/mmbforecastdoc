# Online application

## Result generation

As stated in the previous sections, forecasting results are stored in a `.json` file in each of the folder in estimation. The script `collect_results.py` is to collect all the `.json` files and export them to `results.json` under the path `...\MMB_forecast_application\application\src`

### Model Class register

For each dsge model, please register whether the model is of `Pre-crisis` or `Post-crisis` under `...\MMB_forecast_application\data\ModelClass.xlsx`. This is for the result collecting script to identify which type of model it belongs and hence calculating the mean forecast of the types.
 
### `collect_results.py`

As an overview, the script does the following:

- collect actual GDP growth data
- collect SPF/GB/Fair forecasts data
- collect model forecast estimations
- calculate mean forecast of model acccording to group `Pre-crisis` and `Post-crisis`

Eventually the script export a variable `results` of the type dictionary, which containts all the data needed for graph plotting in the online application. In the dictionary there are five keys

- `actual`: contains the actual data
- `dsge`: contains the forecasting estimation of different dsge models with different vintage dates, also the mean forecast of the models with the name `Post-crisis models avg` and `Pre-crisis models avg` 
- `error`: The forecasting error for the RMSE table
- `external`: external forecasts, such as SPF mean and SPF individual
- `ts`: contains the forecasting estimation of different time-series models with different vintage dates 

The results are then exported to `...\MMB_forecast_application\application\src\results.json`


??? what is `lastforecast.txt`?



## Open the online application

Open visual studio code and install `Live Server`. From the terminal, change your directory to the path `...\MMB_forecast_application\application`. From the explorer inside the VS code, right click the respective html files and click `Open with Live Server`. 