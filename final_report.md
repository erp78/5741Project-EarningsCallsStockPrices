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

We first pre-processed the base earnings table, starting by dropping all the rows with a missing EPS or EPS estimate, since those were important features. This was only a small portion of nearly 170,000 instances. We imputed missing quarter values based on the given date, and missing release times based on previous values for that company. Then we merged the daily stock price table into each row of the earnings table based on the symbol and date. We made sure to account for weekends and offset the previous and next date columns for earnings occurring after Friday market close or Monday before market open, allowing us to retain all stock pricing data for each company on the day of the earnings call and the trading days both before and after the call. We performed another merge to add in the yearly company financial fundamentals data, keeping the metrics we thought were significant. Having completed merging all three tables, we dropped the utility columns that were no longer needed. To account for extreme outliers, we dropped all rows with values with a z-score above 3 for real-valued columns. Then, we dropped all remaining rows with either missing pricing data or values of infinity. Finally, before dropping the stock prices, we used them to define more meaningful features:

- eps_surprise =  (EPS Actual - EPS Estimate) / |EPS Actual|
- pe_ratio = closing price before call / EPS

And our target variables for both regression and binary classification:

- returns = (open(t) - close(t-1))/close(t-1) for a given day t
- log_return = ln(close(t)/close(t-1)) for a given day t
- positive_returns = 1 for positive returns, 0 for negative
- positive_log = 1 for positive log return, 0 for negative

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

We first approached this as a regression problem, trying to predict the percent returns, defined as the percent change between the opening price immediately following an earnings call and the closing price from before the call, multiplied by 100 for easier percentage interpretation. We explored several different variations of linear models, starting with a basic linear regression model. Next, we added in different types of regularization on top of the linear least squares loss function to try to minimize overfitting and reduce variance. We fit a Ridge regression model which uses L2 regularization to penalize large coefficients, a Lasso regression model which uses L1 regularization to minimize the number of non-zero coefficients for sparsity, and an Elastic-Net which combines L1 and L2 regularization. Additionally, we fit a Huber regressor which has the advantage of being robust to outliers, such that the loss function isn’t too heavily influenced by outliers but their effect isn’t completely ignored.

For regression modeling, we utilized the 60/20/20 train-validate-test split. We performed hyperparameter tuning by training the models on the training set, measuring the mean squared error on the validation set, and selecting the hyperparameter values that minimized this. Specifically, we tuned the values of alpha (regularization parameter) for Ridge, Lasso, and Elastic-Net, epsilon (outlier definition parameter) for Huber, and l1_ratio (mixing parameter) for Elastic-Net. After the parameter values were determined, we fit models with these values to the training set, and then evaluated their performance by computing both the train error and the error on the test set. We used root mean squared error and mean absolute error as our metrics.  

### Regression Results

|                                 | Train RMSE | Train MAE | Test RMSE | Test MAE |
|---------------------------------|------------|-----------|-----------|----------|
| LinearRegression                | 5.044229   | 3.426747  | 5.074254  | 3.435696 |
| Ridge (a=0.5)                   | 5.093807   | 3.460001  | 5.048118  | 3.401765 |
| Lasso (a=.001)                  | 5.094912   | 3.460543  | 5.048004  | 3.401033 |
| ElasticNet (a=.001, l1_ratio=1) | same       | as        | lasso     |          |
| Huber (eps=2.35, a=.0017)       | 5.094068   | 3.459318  | 5.048023  | 3.400898 |

Above, we present the results of our regression modeling. Most of the error values across the models were similar. This can be explained by the fact that the scale of our target is very small and the outputs from our models fall in a narrow window. However, regularization did provide slight decreases in error from the non-regularized model. It also improved generalization, as the test errors were no longer greater than the train errors. We conclude that the Lasso and Huber models are best at predicting the test set, which are models that favor sparsity and handle outliers well, respectively.

| <img src="https://user-images.githubusercontent.com/44250480/144774919-14417e17-912b-4a31-880d-21708e4f7ba0.png" width="500"> |
|:--:|
| Fig.6 - Model Coefficients |

Next, we took a look at the coefficients of the trained models to see which features played a significant role in determining the predicted output value. As shown in Fig.6, the non-regularized linear model had a spread of coefficients across several features, with the coefficients largest for liabilities, EPS surprise, assets, and net income. For the remaining three models, EPS surprise and EPS were the features with the largest coefficients, meaning these earnings call-related metrics influenced the predicted output the most. The magnitudes of the coefficients were smaller across the board for Ridge and Huber in comparison to the non-regularized model. And for Lasso, it is clear how sparsity is favored, since only EPS surprise and EPS had non-zero coefficients. The other features didn’t even contribute to the prediction.

| <img src="https://user-images.githubusercontent.com/44250480/144775000-285e2880-fd1c-483a-9589-3c0c691011fb.png" width="800" height="170"> |
|:--:|
| Fig.7 - Actual vs Predicted Returns |

### Classification

After exploring regression, we wanted to also frame the question as a binary classification problem where we try to predict whether the next day returns are positive or negative. This would simply be an indication of whether the stock price increased or decreased immediately following an earnings call. This target variable was defined as the sign of the difference between the opening price immediately following the call and the closing price from before the call. The first type of classification model that we fit to the data was a single decision tree classifier, tuned on max depth. We then built on this by trying some different ensemble methods that combined the predictions of several decision tree base estimators, with the goal of improving generalizability and robustness, and reducing variance. We used a Bagging Classifier which takes the majority vote of the predictions from several bootstrap trees, and a Random Forest, another bagging technique which uses a random selection of features, and handles higher dimensionality data and missing values well. For a boosting method, we tried the Gradient Boosting Classifier with a deviance loss function, which fits and sums consecutive trees. Finally, we fit a Logistic Regression Classifier which uses a probabilistic loss function.

For classification modeling, we utilized the 70-30 train-test split. We performed hyperparameter tuning using 3-fold cross validation on the training set, taking the values that produced the lowest mean ROC-AUC score. For the trees, the primary parameter tuned was max depth, since this practice tends to improve the performance the most, as it is largely dependent on the relation of the inputs. We also tuned max_features and max_leaf_nodes for several of the models to find the optimal level of model complexity. For the logistic classifier, we tuned C (inverse of regularization strength). With the selected values, we trained the classifiers on the training set and evaluated their performance by computing the cross validation scores on the training set and the scores on the test set, using area under the ROC curve and accuracy as our metrics.

### Classification Results

|                                 | Train ROC-AUC | Train Accuracy | Test ROC-AUC | Test Accuracy |
|---------------------------------|:-------------:|:--------------:|:------------:|:-------------:|
| Decision Tree (max_depth=4)     | 0.713234      | 0.669234       | 0.661745     | 0.663346      |
| Bagged Trees                    | 0.723273      | 0.672847       | 0.662544     | 0.664322      |
| Random Forest (max_depth=4)     | 0.715898      | 0.671533       | 0.664060     | 0.665366      |
| Gradient Boosting (max_depth=3) | 0.725771      | 0.672996       | 0.664271     | 0.665924      |
| Logistic Regression (c=.99)     | 0.676365      | 0.620122       | 0.612287     | 0.619602      |

Above, we present the results from our classification modeling. Most of the scores were in the 60s-70s percent range. The ensemble methods improved slightly on the basic tree. There is potential overfitting, since the models performed slightly worse on the test set than the train set, but the difference is minor. We conclude that the Gradient Boosting Classifier is the best model for the data, based on its performance on the test set. This is a model that at every step learns from the previous tree and tries to improve upon that error.

| <img src="https://user-images.githubusercontent.com/44250480/144774508-8a453f98-30cf-482e-8581-3c6fee89ce1c.png" width="500"> |
|:--:|
| Fig.8 - Feature Importance |

Next, we took a look at the feature importance results that the models provided, based on decrease in node impurity. There were strong results across the board. EPS surprise was the most important by a significant amount, with EPS and P/E ratio second and third. This indicates that these earnings call-related features are the most useful at predicting whether the next day returns were positive or negative.

## Modeling Conclusions

After going through the process of fitting initial models, tuning and refining, and comparing results across models, we have come to some conclusions about modeling the problem. The models that performed the best on the test sets, and which we would use to predict future stock price shifts after earnings calls, were Lasso and Huber for Regression and the Gradient Boosted Tree Ensemble for classification. These models were able to generalize the data the best and we are most confident in their ability to make predictions on future data, whether we are interested in the magnitude of the returns or the direction. We observed that when we added in regularization, ensemble techniques, and hyperparameter tuning to our base models, we were able to see slight performance improvements, measured by decreased error for regression or increased accuracy for classification. These improvements were small-scale, which could be due to the small predicted output range or could indicate that there was minimal overfitting to begin with. Most importantly, for both models, we saw that the earnings call-related features influence the predictions the most compared to the other features. They appear consistently among the highest ranked for feature importance and coefficient weights. Without them, it would be difficult to predict the shift in stock price using just the fundamentals features.This leads us to conclude that overall, there is a high correlation between EPS/EPS surprise and the next day returns, but it is tough to always predict this reliably and quantify the exact impact.

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
