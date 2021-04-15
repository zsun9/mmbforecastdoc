# Home

This documentation aims at explaining the programs that are written and used by the team of the MMB forecasting project to 

- collect and transform data,
- estimate models and generate forecasts,
- implement new DSGE models, and
- present results in the online application.

The programs are stored in a private GitHub repository [here](https://github.com/zexisun/MMB_forecast_application).

---

The remainder of the page introduces the theoretical background of the project.

## Basic concepts

`DSGE model`
:   Quantitative economic model used to explain economic growth and business cycles. It contains structural equations that are built on microfoundations and on agents' inter-temporal optimization behaviors.

`Bayesian VAR model`
:   Vector autoregression (VAR) model estimated with Bayesian techniques. The parameters are treated as random variables that have prior distributions. Researchers usually use informative priors to reduce parameter uncertainty. At the moment, we only consider Bayesian VAR models, in which informative priors are optimally chosen according to Giannone, Lenza, and Primiceri (2010).

`Pre-crisis models`
:   Macroeconomic models developed before the 2008-09 financial crisis. These include small-scale NK models, medium-scale NK models, and Cowles Commission type model. The medium-scale model in this group feature nominal and real frictions that are commonly seen in DSGE models, but they don't include any financial frictions.

`Post-crisis models`
:   Macroeconomic models developed after the 2008-09 financial crisis. These models contain real frictions as a result of financing problems faced by firms and households, such as the financial accelerator à la Bernanke, Gertler and Gilchrist (1999), or the collateral constraint à la Kiyotaki and Moore (1997).

!!! note
    The above two groups are actually determined by whether a financial friction is included in the model, rather than by whether the model is published before or after the financial crisis. For example, the Gali, Smets, and Wouters (2012) model is categorized as a pre-crisis model, while the three-equation NK model with a financial accelerator is categorized as a post-crisis model.

`Data vintage`
:   A sample of data collected historically. The word **vintage** is used to highlight the fact that, as many macroeconomic series are revised over time, a specific data point (for example, the real GDP growth in 2008:III) can have different values when it is observed in different time. We employ real-time data vintages for model estimation, meaning that we use data that were observed 

- scenarios

- raw data
- observed data
- ALFRED

- Bayesian estimation
- SPF forecast
- Greenbook forecast
- RMSE

## Forecasting methodology
