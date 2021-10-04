# ORIE 5741 Project Proposal

## Impact of Earnings Calls on Next-Day Common Stock Opening Price

Group Members: Emily Proulx (erp78), Ricky Takkar (rt398), Dan Robinson (dr493)

### Background:

As the team tasked with generating statistical arbitrage-based trading strategies to target and profit off market inefficiencies, we have determined that there is a worthwhile opportunity in investigating the impact of corporate earnings calls on next-day common stock opening prices. Earnings calls are an invaluable tool for investors to remain informed about companies’ performance during the preceding quarter by disclosing their reported earnings-per-share (EPS) figures. Investors, both amateur and institutional, gauge information dispersed by the company executives during an earnings call to make buy and sell decisions related to their assets. If the call took place after the market closed on the previous day, or before market open on the day of, investors have no choice but to wait until the market reopens to execute their buy or sell decisions. We aim to uncover the extent to which a company’s opening price is affected by earnings figures reported during non-trading hours, where opening stock prices tend to jump to reflect updated investor sentiment based on the surprise reported between observed EPS and expected EPS.

### Question:

The question guiding our project is: *To what extent does a company’s earnings call result affect their next day opening price*? By determining this, we would be able to model and predict stock prices post earnings - effectively allowing our team to take a statistically arbitraged position and generate alpha by updating our portfolio ahead of the anticipated stock moves at open the following day. We are confident in our ability to develop this model by looking at historic estimated and reported EPS values from corporate earnings calls and factoring in the stocks prices for the trading day prior to the earnings call, as well as the stocks prices at open for the following trading day.

### Dataset:

We have identified the perfect dataset, [US Historical Stock Prices with Earnings Data](https://www.kaggle.com/tsaustin/us-historical-stock-prices-with-earnings-data), to use for this model. This dataset provides historical stock price data for 7000+ stocks over the past 20 years. It includes two valuable tables, with the first containing daily stock price data consisting of open, intraday low, intraday high, closing prices, and trading volume for each US listed stock. The second table contains earnings data consisting of the date of the companies’ earnings call, whether it was during market close, their estimated EPS going into the calls, and their EPS reported during the calls. This dataset allows us to analyze and predict how the surprise of a companies’ reported EPS versus their estimated EPS affects their share price during the next trading session at market open. With the extent of our data, we expect to complete this project within three months.

