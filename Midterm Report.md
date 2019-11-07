# Midterm Report
## Abstract
In this project, we leveraged our knowledge in dealing with big messy data to examine the statistical correlation between the ratios extracted from the quarterly financial statements and the stock performance of S&P 500 companies within some defined time period after the report release (ie. 1 month after the report release and 2 months after the report release). Further, we will conclude how can we utilize the ratios to serve as trading signals for us to construct an investment portfolio that mitigates the risk to the largest extent and maximize the profit. The upside of the combination of fundamental analysis of the financial reports and technical analysis of the correlations will produce a systematic, reliable and fast method stemming off traditional fundamental and technical analysis.

More specifically, we plan to answer the question “how do we predict the future stock movement based off the quarterly financial statements and how long the prediction would stay effective”. Parameters from the financial statements that are closely related to the change of the stock price are used and are summarized in below sections.

## Data Description
#### Stock prices of S&P 500 companies
We use the financial data provider - Quandl to collect the historical price of each individual company that are listed in the S&P 500 index. We utilized Python and the API provided by Quandl to extract monthly stock price data from 2014-03-31 to 2018-12-31. The data contains the date, open, high, low, close, volume, ex-dividend, split ratio, and adjusted price of different companies. 

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/stock%20return%20histogram.png" width = "350">

During the past five years, the S&P500 Index climbed from 2000 to a historical-high 3080 which generated a 50% return. However, if we look at each individual stock, as shown in the above image, their return ranges from -89% to +310%. Given such a wide range made us believe active investment should give investors opportunities to outperform the market.

#### Quarterly Financial Statement for S&P 500 companies
Again we used Quandl's *US Core Fundamentals* to collect the data from 10-Q reports(Quarterly Financial Statement) of S&P500 companies.
The data set contains 111 columns of key financial indicators. We will discuss what indicators(features) to use and the feature transformation procedure in the below section.
#### Data Cleaning Procedure
##### Missing Values
Since we noticed that around 10 companies were founded after Year 2014, we decided to remove those companies from our analysis scope. After running the API script, we have noticed that 10 companies are corrupted and have only returned a few lines of data. Those companies are excluded from the dataset as well, with tickers: AMCR, BF.B, BRK.B, CPRI, GL, LHX, LIN, LW, EVRG, CDW. We believe removing those companies will not have a significant impact on our trend analysis. 
##### Target Transformation
Since we want to predict the trends of stock price after the release of the financial statement report, we create a target variable which records whether the stock price goes up or down in next quarter. We assign true (1) to goes up and false (0) to goes down.

## Features Description
#### Earnings per Share.
EPS(Earnings per Share) is one of the most important indicators traders and investors may use while examining the financial strength of companies. A high EPS generally means the company is profitable enough to pay out more money to its investors which essentially increases the intrinsic value of the stock of the company. 
EPS = net Income - preferred dividend / average outstanding common shares.

#### Net Profit Growth Rate
NPGR indicates how strong the growth of the company may be in the future, which is often used by investors to predict the future financial performance of the company and make long/short decisions.
NPGR = (Net Profit_current - Net Profit_previous) / Net Profit_previous 

#### Book To Market
BTM (Book to Market) Ratio indicates whether the stock price of the company is overpriced or underpriced which is widely used by investors to determine 
Book to Market = Common Shareholders’ Equity / Market Cap

#### Net Profit Margin
Net profit margin = Net profit ÷ Net sales or revenues

#### Equity Multiplier
Equity Multiplier is a financial leverage ratio that evaluates a company’s efficiency in the usage of debt to purchase assets. 
Equity Multiplier = Total Assets / Stockholder’s Equity

#### return on equity (ROE)
Return on equity is a profitability ratio that measures the ability of a firm to generate profits from its shareholders investments in the company. It shows how much profit each dollar of common stockholder’s equity generates. 
ROE = Net Income / Shareholder’s Equity

#### Free Cash Flow to Equity (FCFE)
FCFE = Net Income - (Capital Expenditures - Depreciation) - (Change in Non-cash Working Capital) -(Preferred Dividends + New Preferred Stock Issued) + (New Debt Issued - Debt Repayments)

#### Features Correlation Analysis
<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/features%20correlation.png" width="350">

From the above correlation matrix, we found that the correlation between EPS and Equity Multiplier is significant, thus we need to consider dropping one of the features or make some adjustment in our further analysis. Apart from this pair, other correlations are not significant.

## Preliminary Analysis and Future Development
#### SVM 
<img src="https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/SVM.png" width = "1000">
As a preliminary analysis, we used the dataset to fit an SVM with Sklearn to do classification. And we are surpriced to find out that the accuracy of the model is over 90% as shown on the graph above. To be more specific, we constructed the confusion matrix and the number on the diagonal added to be close to the size of the data from which we can conclude that we did a fairly good classification. 

#### Feature Selection and Feature Engineering
Upon now, we have just chosen 7 features to be included in our model among over 100 indicators, so in our further analysis, We might use feature selection methods in regression (AIC, BIC or Forward/Backward selection) to find the other feasible features to build the final model. Besides, we may add monthly stock trends instead of quarterly monthly trends which we have currently.

#### Train and Test
We will split our dataset into train set and test set according to the “80-20 rule”. Specifically, we will shuffle the dataset, and use the training set to determine our model. We will then use this model on test set to see whether this model would perform well on new data.
