# SAM Tool
![SAM Tool](./Images/main_image.jpg)

---

## Description
The tool will create a customized asset allocation for an investor to use a reference for an optimal portfolio based on their ability and willingness to take risk. 

---
## Usage
This can be a useful tool for first time investors to assess their investments using a benchmark based on client specific situation. 

---
## Systems Diagram
![Systems Diagram](./Systems_Diagrams/Pipelines/main_pipeline.png)

---
## Installation Guides / Import libraries and dependencies

* import pandas as pd
* import numpy as np
* import os
* from dotenv import load_dotenv
* import alpaca_trade_api as tradeapi
* import datetime as dt
* import pytz
* from pytz import timezone
* import sys
* import fire
* from pathlib import Path
* %matplotlib inline
* import hvplot.pandas

---
## User Profile
This is will collect financial information to categorize investor into a risk profile. Initial screen will prompt dialog to retrieve investor's financial information to determine risk profile. Based on information provided, user will be promted to next steps or give an output of "insufficient funds" if investor does not have sufficient funds to invest. 

### Determine Investor's Net Worth
Investor's net worth is defined by sum of cash, assets, and annual income minus annual spending needs

    + Cash
    + Assets (i.e. stocks, bonds, real estate, cryptoasset)
    + Annual Income
    - Annual Spending Needs (ie. rent/mortgage, credit card, loan payments)

### Determine Investor's Risk Ability
Investor's ability to take risk is determined by their investment time horizon which is the number of years they can invest. We assume an average investor's retirement age will be 65. Investment time horizon will be determined by 65 minus age of investor.

    * If less than 5 years to invest, assign a value of 1
    * If between 6 and 10 years to invest, assign a value of 2
    * If more than 10 years to invest, assign a value of 3

### Determine Investor's Risk Willingness
Investor's willingness to take risk is a preference and determined by their level of comfort with loss.  

    * If investor is comfortable with losing at most 10%, assign a value of 1
    * If investor is comfortable with losing 10% to 50%, assign a value of 2
    * If investor is comfortable with losing more than 50%, assign a value of 3

### Score Questionnaire
Average the score from ability and willingness to take risk. Assign each score as a risk profile:

    * 1 = Conservative
    * 1.5 = Moderately Conservative
    * 2 = Moderate
    * 2.5 = Moderately Aggressive
    * 3 = Aggressive

Each risk profile assigns different allocation to risk assets such as stocks and crypto to a more conservative asset like bonds. For example, we assigned higher weighting to riskier assets like crypto and stocks for an aggressive profile and vice versa for conservative investor. 

![SPY AGG BTC df](./Images/SPY_AGG_BTC_df.png)

### Gather, clean, consolidate Dataframe for SPY, AGG, BTC
Resources: 
=GOOGLEFINANCE(ticker, [attribute], [start_date], [end_date|num_days], [interval]). 
 * ticker = 'SPY', 'AGG', 'CURRENCY:BTCUSD'
 * attribute = 'close'
 * start_date = 12/31/2010
 * end_date = 12/31/2020

Close prices between 12/31/2010 to 12/31/2020 gathered from GOOGLEFINANCE and exported as CSV. We used SPY ETF (S&P 500 Index) as a proxy for stocks, AGG ETF (US Aggregate Bond Index) as a proxy for bonds, and Bitcoin as a proxy for crypto assets. The dataframe is limited to 11/20/2015 to 12/28/2020 due to lack of historical data for Bitcoin only dating to 11/20/2015 in GOOGLEFINANCE

    * SPY.csv (close price, timeframe = 11/20/2015 - 12/31/2020)
    * AGG.csv (close price, timeframe = 12/31/2010 - 12/31/2020)
    * BTC.csv (close price, timeframe = 12/31/2010 - 12/31/2020)

* Plot daily return
combined_daily_return = combined_df.pct_change().dropna()
![daily return](./Images/asset_daily_return_hvplot.png)

* Plot cumulative return for all asset classes
combined_cumulative_return = (1 + combined_daily_return).cumprod() - 1
![cumulative return](./Images/asset_cumulative_return_plot.png)

---
## Calculations
Based on investor's profile, calculate the portfolio's average annualized return, average annual volatility and Sharpe Ratio for optimal risk-adjusted return. We will assume the following asset allocation for each profile:

![Risk Profiles](./Images/risk_profile.png)


---
## Output / Visualizations
When we look at Sharpe Ratio, which is the risk adjusted return, we find that the most aggressive profile has the highest risk adjusted return but a moderately conservative portfolio had higher risk adjusted return than a moderate portfolio which is deemed to be riskier. While higer risk results in higher returns, this goes to show the value of diversification over different market environments.

* Based on each user's input and risk profile, display Portfolio return, volatility, sharpe ratio in a table.

![Risk_Profile_Table](./Images/risk_profile_table.png)
![Risk_Profile_Sharpe](./Images/risk_profile_sharpe_barchart.png)
![Risk_Profile_Sharpe](./Images/risk_profile_return_barchart.png)
![Risk_Profile_Sharpe](./Images/risk_profile_vol_barchart.png)

* optimal portfolio return, volatility, sharpe ratio based on efficient frontier
* Specific ticker against portfolio/benchmark
* Monte Carlo Simulation

---
## Contributors

_Sarah Kang_ 

_Might Lee_  

_Aaron Montano_

---

## License

