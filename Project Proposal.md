# Project Proposal - Stock Price Prediction
#### Haozhe Wang(hw672) Ruoyu Chen(rc855) Xia Wang(xw563)

## Background
There are two basic methods of predicting stock price, namely fundamental analysis and technical analysis. In the finance industry, the common practice for fundamental analysis on an efficient market is to analyze the financial statements and make predictions of the future stock price to make long-term investment decisions. Opposing to the analysis of the intrinsic value of the company, analysts assume the future return is a reflection of the stock price patterns observed.
 
In this project, we would like to leverage our knowledge in dealing with big messy data to examine the statistical correlation between the financial statement and the stock performance, and how it serves as a trading signal for us to construct an investment portfolio that mitigates the risk to the largest extent and maximize the profit. The upside of the combination of financial analysis and technical analysis will produce a systematic, reliable and fast method stemming off traditional fundamental and technical analysis.

More specifically, we plan to answer the question “how do we predict the future stock price based off the quarterly financial statements and how long the prediction would stay effective”. Parameters from the financial statements which will be used are: earning per share (EPS), book value per share (BPS), and net profit growth rate (NPGR), as those parameters are closely related to the change of the stock price. 

## Data
The scope of the project is limited to the quarterly financial statements of S&P 500 companies and their stock prices from year 2009 to the present. The quarterly financial statements used for this project will be extracted from SEC’s database system EDGAR and the historical stock prices will be extracted from Yahoo Finance or Bloomberg.

## Methods
In this project, we will use the following machine learning techniques: 
Firstly, we will use feature engineering to transform information collected from financial statement to datasets that are analyzable. Then, we will use linear regression, SVMs and random forest to classify (i.e. Whether the stock price will rise or fall in response to different patterns shown on the financial statements). In addition, we may use regression analysis and time series analysis to further improve our models quantitatively. Lastly, we will use the clustering method to construct an investment portfolio as per the Markowitz Theory, adjusting the parameters based on the backtests.

