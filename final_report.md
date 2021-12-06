# ORIE 5741 Final Report: Impact of Earnings Calls on Next-Day Common Stock Opening Price

Group Members: Emily Proulx (erp78), Ricky Takkar (rt398), Dan Robinson (dr493)

## Introduction

The stock market is currently at its most accessible point in human history. Anyone from teenagers still in high school to adults with little more than an internet connection can now trade nearly to their heart’s desire. It is important for market participants to understand market dynamics and what they are up against. One such intricacy that we will be investing is the impact of a company’s earnings call on their stock price.

We are looking exclusively at the stock market in the US. By law, the Securities and Exchange Commission requires public companies listed on US stock exchanges to report quarterly and annual updates on their business to shareholders, called an earnings release. Company leadership releases their key performance metric: earnings per share (EPS), as well as updates to their revenue and profit forecast, and various updates to their business. Investors, both amateur and institutional, gauge this dispensed information to make buy and sell decisions related to their assets.

Overwhelmingly, but not always, these earnings calls take place outside of market hours to mitigate highly volatile trading and maintain market stability; investors have to wait until the market reopens to execute their buy or sell decisions. As a result, most of these swings are delayed until the open of the next trading session following the earnings call. The magnitude of which is what our project aims to predict. Specifically, we try to uncover the extent to which a company’s earnings call affects their next day opening stock price. Answering this question opens the door to developing profitable trading strategies for any market participant, as well as having further reaching implications for trading strategies in the equity derivatives market.

## Data

### Datasets

The final dataset we analyzed consisted of nearly 48,000 trading days immediately following a company’s earnings call for every US listed stock over the past decade. We found three datasets on Kaggle scraped from Nasdaq, Yahoo Finance, Zacks, and Alpha Vantage. The first dataset consisted of daily stock price data. The second contained quarterly data about every stock’s earnings calls. The third table contained a wide variety of company fundamentals for every US listed company in a given year. Below is a table of the 23 features we decided to keep for our models beyond the stock ticker and dates, including two features, eps_surprise and pe_ratio, that we added ourselves:

| Model Feature |                      Value Represented                     |   |  Model Feature |            Value Represented            |
|:-------------:|:----------------------------------------------------------:|---|:--------------:|:---------------------------------------:|
|  eps_surprise |        (EPS Actual - EPS Estimate) / \|EPS Actual\|        |   |   investments  |               Investments               |
|    pe_ratio   |            Stock Closing Price Before Call / EPS           |   |   liabilities  |            Total Liabilities            |
|      eps      |                  Earnings per Basic Share                  |   |    marketcap   |          Market Capitalization          |
|      vol      |                       Trading Volume                       |   |     ncfdebt    | Issuance (Repayment) of Debt Securities |
|     assets    |                        Total Assets                        |   |     netinc     |                Net Income               |
|      cor      |                       Cost of Revenue                      |   |       pe       |    Price Earnings (Damodaran Method)    |
|      debt     |                         Total Debt                         |   |       ps       |      Price Sales (Damodaran Method)     |
|    divyield   |                       Dividend Yield                       |   |     revenue    |                 Revenues                |
|     ebitda    | Earnings Before Interest Taxes & Depreciation Amortization |   |    shareswa    |         Weighted Average Shares         |
|      fcf      |                       Free Cash Flow                       |   | workingcapital |             Working Capital             |
|       gp      |                        Gross Profit                        |   |       qtr      |              Fiscal Quarter             |
|   inventory   |                          Inventory                         |   |                |                                         |

### Data Preparation and Cleaning

### Data Visualizations



| <img src="https://user-images.githubusercontent.com/44250480/144770852-9a158106-c6f4-4b7d-ae26-70863e592fec.png" width="500"> |
|:--:|
| Fig.1 - Correlation Heat Map |
Fig.1 depicts a correlation matrix to represent the correlation between different features using a colored scale where lighter colors show higher correlation and darker colors show lower correlation. This provides insight into which features are related so we can keep in mind any potential multicollinearity issues when modeling.

| <img src="https://user-images.githubusercontent.com/44250480/144770953-2ef56d5b-4466-44e1-b71f-9ebb4910864f.png" width="650"> |
|:--:|
| Fig.2 - Frequency of Post-Earnings Call Returns |
Fig.2 contains two histograms showing the distribution of our two regression targets - post-earnings call log return and returns. This allows us to visualize the range of the values we are trying to predict. The log plot above reveals that the frequencies lie in a symmetrical fashion around 0. 

| <img src="https://user-images.githubusercontent.com/44250480/144771045-20974638-dd7e-4bd2-80d3-9e791e788995.png" width="650"> |
|:--:|
| Fig.3 - Return vs EPS Surprise and Return vs P/E Ratio |
The above figures contain scatter plots of our targets (log return and returns) versus two of our features of interest (EPS surprise and P/E ratio). The plots don’t suggest a strong direct linear relationship between the variables, and they indicate some outliers.

| <img src="https://user-images.githubusercontent.com/44250480/144771089-62a28449-9b80-4871-bc61-edf027a2584d.png" width="450"> |
|:--:|
| Fig.4 - EPS vs Estimated EPS |
Fig.5 shows a scatter plot for EPS against the estimated EPS where red dots represent negative returns and blue dots represent positive returns. This visualizes the classification task. While the labels are interspersed across the plot, it does seem as though there are more blue points on top of the diagonal where EPS is greater than estimated EPS, and more red points below the diagonal where EPS is less than estimated EPS. This would support the claim that there’s a correlation between a positive EPS surprise and a rise in stock price.

## Modeling & Analysis

### Plan for Developing and Testing Models

| <img src="https://user-images.githubusercontent.com/44250480/144771435-927719f2-fd6c-46bd-bbb1-c4d2aa3b0e4e.png" width="800"> |
|:--:|
| Fig.5 - Modeling Approach Timeline |
The timeline above is labeled numerically in chronological order outlining each step of our modeling approach. 

### Regression

### Regression Results

### Classification

### Classification Results

## Modeling Conclusions

<BODY>

## Discussion
  
### Fairness

We concluded that fairness was not an issue affecting our model. First, our dataset consisted of zero protected attributes: all the features used were stock-related, publicly available and unrelated to any form of discrimination thus mitigating uninted effects on output from other covariates (proxies). Second, the timeframe of our dataset was of the past twelve years during which the global economy and US stock market experienced several ups and downs so such data wouldn’t lend itself to feature biased expansionary or reductionary outlooks. 

### Future Improvements

There are two key ways we can improve on our models in the future. The first is by clustering our stocks into similar groups by either sector or classification. Stocks within the same sector tend to have similar trading patterns as investors trade them more similarly to other stocks in their same industry than stocks in the broader market. Stocks can also be traded differently due to the reasons investors have for purchasing them in the first place. Based on investor motivation stocks can be partitioned into seven categories: blue chip, speculative, growth, value, income, cyclical, and penny stocks. Applying our models to such clusters as opposed to the market as a whole would likely bring more meaningful results, though it is hard to find accurate datasets organized in such a manner over a long time horizon.   
 
Another way to improve our models is by analyzing earnings beyond the EPS figures to factor in changes to revenue and expense forecasts, and by classifying the earnings call as positive or negative for the company as a whole. While EPS is always reported on the earnings calls, the other metrics may not be as consistently known, making data collection more difficult. To classify calls as positive or negative, the industry has turned to automatically scraping live transcripts of the calls and classifying the text itself as positive or negative. This is a more recent practice and takes significant computing power on a large scale, so while it is not practical for analyzing historical data, it may well be in the future.

## References
  
1. Mathur, A. (2019, March 8). Companies Fundamentals Us. Kaggle. Retrieved December 5, 2021, from https://www.kaggle.com/armathur/companies-fundamentals-us.
2. QuantRocket LLC. (2019, March 3). Sharadar US Fundamentals Data Guide. QuantRocket. Retrieved December 5, 2021, from https://docs-1-8--quantrocket.netlify.app/docs/data/fundamental/sharadar/#about-sharadar.
3. TS. (2021, June 15). US historical stock prices with earnings data. Kaggle. Retrieved December 5, 2021, from https://www.kaggle.com/tsaustin/us-historical-stock-prices-with-earnings-data. 
