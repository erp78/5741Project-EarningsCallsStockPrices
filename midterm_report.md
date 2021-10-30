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

### Avoid Over and Under Fitting

To avoid overfitting and underfitting, we plan to use a simple model of at most a 5th order linear regression, as well as utilizing regularization to prevent overly large coefficients having an outsized impact on the models. 

### Testing the Effectiveness of Our Models

To test the effectiveness of our models, we will partition the data into training and test sets of multiple sizes, playing around with between 15-30% training data and 70-85% test data. By doing this, we can validate our model using the test data and generalize it to predict future stock price movements based on earnings momentum.

### Histograms and Descriptive Statistics of the Data (unfinished)
- Look at frequency of stock prices that increase/decrease after earnings call (you could make two histograms with this: one for increase and one for decrease)
- Histogram for distribution of |eps - eps_estimated|

### Features and Examples (unfinished)
Our two datasets have a combined 15 features between. Before cleaning the data, they had a combined 220 million elements, with 24 million examples, where each example has the features detailed in the above description of our dataset for a single stock on a single trading day. Because each dataset starts from vastly different dates of over eleven years, we will disregard the first eleven years of historical stock prices in the dataset. Since we are also only interested in prices before, during, and after earnings release, we will disregard all examples with stock price data outside of those dates, drastically reducing the size of our data to a combined…… examples/elements.

### Corrupted or Missing Data (unfinished)
Utilizing Panda’s isna() and value_counts() functions, we found that the stock prices table had no missing or corrupted data, but the earnings table did for the quarter, estimated eps, eps, and release time columns. We first decided to drop all of the rows with missing data for eps or estimated eps, since these are the main features whose effects we are interested in analyzing. This resulted in a drop from 168,603 to 110,994 rows of data. Next, for the 30,548 rows that were missing a release time, we filled in a value of “during”, since these earnings calls took place neither pre- nor post- market. For the 1004 missing values in the quarter column, we inferred from the date column. We also reformatted all of the values to a standard form, since it was previously malformed, with some in a “Q1” format and others in a “MM/YYYY” date format.

### Preliminary Analyses (unfinished)

### Further Steps (unfinished)