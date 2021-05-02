# Home

This documentation aims at explaining the programs that are written and used by the team of the MMB forecasting project to 

- collect and transform data,
- estimate models and generate forecasts,
- implement new DSGE models, and
- present results in the online application.

The programs are stored in a private GitHub repository [here](https://github.com/zexisun/MMB_forecast_application).

!!! info
    This version of the documentation is prepared by KaiLong Liu and Zexi Sun. Last updated: May 3rd, 2021.

The remainder of the page introduces the theoretical background of the project.

---

## Core concepts

`DSGE model`
:   Quantitative economic model used to explain economic growth and business cycles. It contains structural equations that are built on micro-foundations and on agents' inter-temporal optimization behaviors.

`Bayesian VAR model`
:   Vector auto-regression (VAR) model estimated with Bayesian techniques. The parameters are treated as random variables that have prior distributions. Researchers usually use informative priors to reduce parameter uncertainty. At the moment, we only consider Bayesian VAR models, in which informative priors are optimally chosen according to Giannone, Lenza, and Primiceri (2010, GLP hereafter).

`Pre-crisis models`
:   Macroeconomic models developed before the 2008-09 financial crisis. These include small-scale NK models, medium-scale NK models, and Cowles Commission type model. The medium-scale model in this group feature nominal and real frictions that are commonly seen in DSGE models, but they don't include any financial frictions.

`Post-crisis models`
:   Macroeconomic models developed after the 2008-09 financial crisis. These models contain real frictions as a result of financing problems faced by firms and households, such as the financial accelerator à la Bernanke, Gertler and Gilchrist (1999), or the collateral constraint à la Kiyotaki and Moore (1997).

!!! note
    The above categorization is actually determined by whether a financial friction is included in the model, rather than by whether the model is published before or after the financial crisis. For example, the Gali, Smets, and Wouters (2012) model is categorized as a pre-crisis model, while the three-equation NK model with a financial accelerator is categorized as a post-crisis model.

`Vintage data`
:   A sample of data collected historically. The word **vintage** is used to highlight the fact that, as many macroeconomic series are revised over time, a specific data point (for example, the real GDP growth in 2008:III) can have different values when it is observed in different points in time. We use real-time data vintages for model estimation, meaning that the data we use are the same as the data that forecasters faced in real time.

`Bayesian estimation`
:   The method for estimating the parameters $\theta$ of the DSGE and VAR models. Given data $D$, it combines the prior densities of the parameters $p(\theta)$ with the likelihood $p(D|\theta)$ to obtain the posterior densities $p(\theta|D) \propto p(\theta) p(D|\theta)$, up to a normalizing constant. We employ the Metropolis-Hasting (MH) algorithm to draw samples from the posterior densities to calculate the forecast.

`SPF forecast and Greenbook forecast`
:   External sources that we compare the model-based forecasts with. One is the Survey of Professional Forecasters (SPF), which is a quarterly survey of macroeconomic forecasts conducted by the Federal Reserve Bank of Philadelphia. Another is the Greenbook, which is produced by the Federal Reserve Board of Governors before each meeting of the Federal Open Market Committee. The second is available to the public with a five-year lag.

---

## Forecasting methodology

### The objective

Obtain the nowcast and one- to-four-step-ahead forecasts of the U.S. real GDP growth rate

### Models

GDP forecasts are calculated based on the estimation results of the following models

=== "Pre-crisis DSGE models"

    - `DS04`: Del Negro and Schorfheide (2004)
    - `WW11`: Wieland and Wolters (2011)
    - `SW07`: Smets and Wouters (2007)
    - `FU20`: Fratto and Uhlig (2020)
    - `FRBEDO08`: Edgee et al. (2008)
    - `GSW12`: Gali, Smets, and Wouters (2012)

=== "Post-crisis DSGE models"

    - `NKBGG`: Bernanke, Gertler, and Gilchrist (1999)
    - `IN10`: Iacoviello and Neri (2010)
    - `CMR14`: Christiano, Motto, and Rostagno (2014)
    - `DNGS15`: Del Negro, Giannoni, and Schorfheide (2015)
    - `KR15_FF`: Kolasa and Rubaszek (2015)
    - `KR15_HH`: Kolasa and Rubaszek (2015)
    - `QPM08`: Carabenciov et al. (2008)

=== "Bayesian VAR models"

    - `GLP3v`: Bayesian VAR with GLP priors, same observables as DS04
    - `GLP5v`: Bayesian VAR with GLP priors, same observables as NKBGG
    - `GLP8v`: Bayesian VAR with GLP priors, same observables as DNGS15

### Data scenarios

The models are estimated in the following four scenarios, which are different in how the current-quarter data are included in the sample. 

- In the first scenario, current-quarter data are not available.
- In the second scenario, the nowcast from the Survey of Professional Forecasters (SPF) are available.
- In the third scenario, current-quarter data are available for some observables, provided that they are updated on a monthly or higher frequency
- In the fourth scenario, current-quarter data from both the second and third scenarios are available.

### Data vintages

To analyze the forecasting performance of the models during economic recessions, we estimate models using data constructed in the following quarters

- `2001:I-2001:IV` for the 2001 recession
- `2008:III-2009:II` for the Great Recession of 2008-2009
- `2020:I-2020:IV` for the COVID-19 recession of 2020

We adopt a rolling window strategy to fix the number of quarters in each sample to 100.

To make sure that the the information set used for estimation are perfectly aligned with the information available to professional forecasters who take the SPF, we retrieve the data available at **the submission deadline for the SPF** in each quarter. It influences the current-quarter data that are included in the third scenario most.

!!! example
    The SPF's submission deadline for 2008:III was August 7th, 2008. Thus, the real time data constructed for this quarter contains all the information released up till Aug 7th. It contains the real GDP growth in 2008:II, as it is published on July 31st. This observation is available in all the scenarios. It contains the effective federal funds rate in 2008:III, as it is updated on a daily frequency. We use its mean from Jul 1st - Aug 7th as its value in 2008:III. Note however that this observation is only available in the third and fourth scenarios.

### The algorithm

- Obtain the maximum likelihood (ML) estimate of the parameters
- Run the MH algorithm with 1,000,000 replications. Set the mean of the first proposal distribution to the ML estimate. Set the scale factor to ensure the acceptance rate is between 20% and 40%. Drop the first 30% of the draws.
- Draw 5,000 samples from the above 1,000,000 replications to calculate forecasts

!!! note
    It usually takes less than 10 hours for one estimation to be done on a computer with i7 8th generation (or higher) CPU. The only exception is the CMR14 model, which takes much longer time to estimate than any other models. Therefore, we only draw 100,000 samples for this model.
