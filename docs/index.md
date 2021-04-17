# Home

This documentation aims at explaining the programs that are written and used by the team of the MMB forecasting project to 

- collect and transform data,
- estimate models and generate forecasts,
- implement new DSGE models, and
- present results in the online application.

The programs are stored in a private GitHub repository [here](https://github.com/zexisun/MMB_forecast_application).

---

The remainder of the page introduces the theoretical background of the project.

## Core concepts

`DSGE model`
:   Quantitative economic model used to explain economic growth and business cycles. It contains structural equations that are built on microfoundations and on agents' inter-temporal optimization behaviors.

`Bayesian VAR model`
:   Vector autoregression (VAR) model estimated with Bayesian techniques. The parameters are treated as random variables that have prior distributions. Researchers usually use informative priors to reduce parameter uncertainty. At the moment, we only consider Bayesian VAR models, in which informative priors are optimally chosen according to Giannone, Lenza, and Primiceri (2010).

`Pre-crisis models`
:   Macroeconomic models developed before the 2008-09 financial crisis. These include small-scale NK models, medium-scale NK models, and Cowles Commission type model. The medium-scale model in this group feature nominal and real frictions that are commonly seen in DSGE models, but they don't include any financial frictions.

`Post-crisis models`
:   Macroeconomic models developed after the 2008-09 financial crisis. These models contain real frictions as a result of financing problems faced by firms and households, such as the financial accelerator à la Bernanke, Gertler and Gilchrist (1999), or the collateral constraint à la Kiyotaki and Moore (1997).

!!! note
    The above categorization is actually determined by whether a financial friction is included in the model, rather than by whether the model is published before or after the financial crisis. For example, the Gali, Smets, and Wouters (2012) model is categorized as a pre-crisis model, while the three-equation NK model with a financial accelerator is categorized as a post-crisis model.

`Data vintage`
:   A sample of data collected historically. The word **vintage** is used to highlight the fact that, as many macroeconomic series are revised over time, a specific data point (for example, the real GDP growth in 2008:III) can have different values when it is observed in different points in time. We use real-time data vintages for model estimation, meaning that the data we use are the same as the data that forecasters faced in real time.

`Data scenarios`
:   Refers to how the current-quarter data are inclunded in the sample. In the first scenario, current-quarter data are not available. In the second scenario, the nowcast from the Survey of Professional Forecasters (SPF) are available. In the third scenario, current-quarter data are available for some observed variables, provided that they do not have publication lags. In the fourth scenario, both the current-quarter nowcast from the SPF and current-quarter observations from those variables without publication lags are available.

`Bayesian estimation`
:   The method for estimating the parameters $\theta$ of the DSGE and VAR models. Given data $D$, it combines the prior densities of the parameters $p(\theta)$ with the likelihood $p(D|\theta)$ to obtain the posterior densities $p(\theta|D) \propto p(\theta) p(D|\theta)$, up to a normalizing constant. We employ the Metropolis-Hasting (MH) algorithm to draw samples from the posterior densities to calculate (the nowcast and) the forecast.

`SPF forecast and Greenbook forecast`
:   To evaluate the performance of the model-based forecast, we compare them with the forecasts from two external sources. One is from the Survey of Professional Forecasters (SPF), which is a quarterly survey of macroeconomic forecasts conducted by the Federal Reserve Bank of Philadelphia. Another is from the Greenbook, which is produced by the Federal Reserve Board of Governors before each meeting of the Federal Open Market Committee. The second is available to the public with a five-year lag.

## Forecasting methodology

- Objective: Obtain the nowcast and one- to-four-step-ahead forecasts of the U.S. real GDP growth rate
- Models:
- Data vintages:
- Data scenarios:
- Algorithm: 
    * Obtain the maximum likelihood (ML) estimate of the parameters
    * Run the MH algorithm with 1,000,000 replications. The mean of the first proposal distribution equals the ML estimate. The scale factor of the proposal distribution is set individually to ensure that the acceptance rate is between 20% and 40%. The fraction of initially generated samples to be dropped is set to 30%.
    * Draw 5,000 samples from the above 1,000,000 replications to calculate nowcast and forecasts

!!! note
    It should take less than 10 hours for one estimation to be done on a computer with i7 8th generation (or higher) CPU. The only exception is the Christiano, Motto, and Rostagno (2014, hereafter CMR14) model, which takes much longer time to estimate than any other models. Therefore, we only draw 100,000 samples for the CMR14 model.

## Important readings

Del Negro, M., Schorfheide, F., 2013. DSGE model-based forecasting. Handbook of Economic Forecasting, 2A: 57-140. [Link](https://www.sciencedirect.com/science/article/pii/B9780444536839000025)

Wieland, V., Wolters, M., 2011. The diversity of forecasts from macroeconomic models of the US economy. Economic Theory, 47(2-3): 247-292. [Link](https://link.springer.com/article/10.1007/s00199-010-0549-7)

Wieland, V., Wolters, M., 2013. Forecasting and policy making. Handbook of Economic Forecasting, 2A: 239-325. [Link](https://www.sciencedirect.com/science/article/pii/B9780444536839000050)

Wolters M, 2015. Evaluating point and density forecasts of DSGE models. Journal of Applied Econometrics, 30(1): 74-96. [Link](https://onlinelibrary.wiley.com/doi/abs/10.1002/jae.2363)

