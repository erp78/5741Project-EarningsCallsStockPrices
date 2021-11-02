# ORIE 5741 Midterm Report

## Impact of Earnings Calls on Next-Day Common Stock Opening Price

Group Members: Emily Proulx (erp78), Ricky Takkar (rt398), Dan Robinson (dr493)

### Description of the Dataset

Every publicly listed US company is required by the SEC to publish a report detailing their earnings for that quarter. Our dataset consists of daily price data and quarterly earnings data for every stock listed on US exchanges dating back over a decade. It’s split into two tables, one for daily price information, and one for quarterly earnings information. The daily price dataset has data from January 2, 1998 through June 14, 2021 with each row of the dataset being a snapshot of one day of a single stock. The earnings dataset has data from April 16, 2009 through June 14, 2021 with each row being a snapshot of a single earnings release for a single stock. The stock price data is compared to the earnings release data in a summarized table below: 
| Stock Price Data       | Earnings Release Data |
|------------------------|-----------------------|
| Stock Ticker           | Stock Ticker          |
| Date                   | Date Reported         |
| Open Price             | Quarter Reporting For |
| Highest Price          | EPS Estimate          |
| Lowest Price           | EPS Actual            |
| Closing Price          |                       |
| Adjusted Closing Price |                       |
| Volume                 |                       |
| Split Coefficient      |                       |

### Plan for Developing and Testing Models

To avoid overfitting and underfitting, we plan to use a simple model of at most a 5th order linear regression, as well as utilizing regularization to prevent overly large coefficients having an outsized impact on the models. 

To test the effectiveness of our models, we will partition the data into training and test sets of multiple sizes, playing around with between 70-85% training data and 15-30% test data. By doing this, we can validate our model using the test data and generalize it to predict future stock price movements based on earnings momentum.

### Histograms and Descriptive Statistics of the Data

| ![image1](https://user-images.githubusercontent.com/44250480/139758501-0ee9b9bb-861f-46f1-bfbd-510271839984.png) |
|:--:|
| Fig.1 - Returns |
Fig.1 shows the distribution of stock price returns at market open the day following an earnings release. We capped the next-day returns at a 10% price change, as the tails become much less informative for larger price changes due to their increased rarity. 

| ![image2](https://user-images.githubusercontent.com/44250480/139758526-0bb0df22-07de-448d-8a36-480d408c6985.png) |
|:--:|
| Fig.2 - EPS Surprise |
Fig.2 shows the distribution of the differences between the EPS’ reported during earnings releases and their estimates beforehand. We capped the difference at a 100% surprise, as the tails become much less informative for differences due to their increased rarity. We can see the distribution looks very similar to the returns above, confirming our prediction that they are strongly correlated.

This [linked](https://postimg.cc/G9Kzd7tv) figure shows the descriptive statistics of our cleaned data. It consists of the mean, standard deviation, extreme values and quartiles for the EPS figures and estimates, as well as important stock prices for each of our 101,174 examples.

### Features and Examples

Our two datasets have a combined 15 features. Before cleaning the data, they had a combined 220 million elements, with 24 million examples, where each example has the features detailed in the above description of our dataset for a single stock on a single trading day. Because each dataset starts from vastly different dates of over eleven years, we will disregard the first eleven years of historical stock prices in the dataset. Since we are also only interested in prices before, during, and after earnings release, we will disregard all examples with stock price data outside of those dates. To combine the data, we performed three join operations to add, for each row in the earnings table, all of that company’s stock price data for the day of the call, the day after, and the day before (7 features each). This reduced the size of the combined data to 110,174 rows with 33 features each, for a total of 3,338,742 elements.

### Corrupted or Missing Data

The stock prices table had no missing or corrupted data, but the earnings table did for the quarter, estimated eps, eps, and release time columns. To handle this, we first decided to drop all of the rows with missing data for eps or estimated eps, since these are the main features whose effects we are interested in analyzing. Next, for the 1004 missing values in the quarter column, we used the date column to infer this. We also reformatted all of the values to a standard form (1, 2, 3, 4), since it was previously malformed with some in a “Q1” format and others in a “MM/YYYY” date format. For the 30,548 rows that were missing a release time, we noticed that many were recent rows for the earnings calls that happened within the past two years. For these, we filled in the company’s most recent non-null release time. For companies who had no previous release times listed, we filled in “unknown.”

### Preliminary Analyses

<img width="411" alt="image1" src="https://user-images.githubusercontent.com/44250480/139779274-dae05cba-8ba4-4e66-af19-d128c2f053be.png">

Fig.3 - MSEs with Least Squares Regression

The primary question we are aiming to answer is: To what extent does a company’s EPS announced during their earnings call affect their next day opening price? In our preliminary analyses, we used a least-squares regression model fitted to 70% of the shuffled data frame to get a truly random train/test split. The data matrix X consisted of: (1) opening price on the day of the earnings call, (2) EPS surprise (difference between the EPS’ reported during earnings releases and their estimates beforehand), and (3) closing price on the day of the earnings announcement. The target vector y consisted of the opening price on the day after the earnings call. Fig.3 shows the predicted labels versus the actual labels (we only plot the first 10,000 points to avoid slow plots) along with the computed train and test mean-squared error.


### Further Steps

As shown in the plot above, the current model has a train MSE of 13.94 and a test MSE of 14.58. This is already quite a low MSE: it should not be a surprise that in the stock market, most companies’ share value is not likely to change dramatically overnight no matter the earnings result. Furthermore, it would be beneficial to factor in earnings release time (pre/post) when thinking about what the “next day” is as opposed to merely looking at trading hours. To further reduce the MSE, we aim to add more features into our model like close price and trading volume. Moreover, we believe it would benefit us to introduce regularization in our models to reduce variance, impose prior structural knowledge and improve interpretability while keeping in mind to minimize the unintended increase of bias.   
