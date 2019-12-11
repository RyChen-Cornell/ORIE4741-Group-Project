# Stock Price Prediction Final Report



**ORIE4741 Learning With Big Messy Data**

**Instructor: Prof. Udell**





Haozhe Wang     hw672

Ruoyu Chen       rc855










# Problem Description

There are two basic methods of predicting stock price, namely fundamental analysis and technical analysis. In the finance industry, the common practice for fundamental analysis on an efficient market is to analyze the financial statements and make predictions of the future stock price to make long-term investment decisions. Opposing to the analysis of the intrinsic value of the company, analysts assume the future return is a reflection of the stock price patterns observed.

In this project, we would like to leverage our knowledge in dealing with big messy data to examine the statistical correlation between the financial statement and the stock performance, and how it serves as a trading signal for us to maximize the profit. The upside of the combination of financial analysis and technical analysis will produce a systematic, reliable and fast method stemming off traditional fundamental and technical analysis.

More specifically, we plan to answer the question &quot;how do we predict the future stock trend based off the quarterly financial statement&quot;. To achieve this, we formularized this problem as a classification problem, that is whether the stock price will go up or down corresponding to different feature patterns.

# Data Set Description

## Stock prices of S&amp;P 500 companies

We use the financial data provider - Quandl to collect the historical price of each individual company that are listed in the S&amp;P 500 index. We utilized Python and the API provided by Quandl to extract monthly stock price data from 2014-03-31 to 2018-12-31. The data contains the date, open, high, low, close, volume, ex-dividend, split ratio, and adjusted price of different companies.

During the past five years, the S&amp;P500 Index climbed from 2000 to a historical-high 3145 which generated a 50% return.

##

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/sp500.png" width = "1000">

However, if we look at each individual stock, as shown in the below image, their return ranges from -89% to +310%. Given such a wide range made us believe active investment should give investors opportunities to outperform the market.

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/stock%20return%20histogram.png" width = "350">


## Quarterly Financial Statement for S&amp;P 500 companies

Again we used Quandl&#39;s _US Core Fundamentals_ to collect the data from 10-Q reports(Quarterly Financial Statement) of S&amp;P500 companies. The data set contains 111 columns of key financial indicators. We will discuss what indicators(features) to use and the feature transformation procedure in the below section.

# Data Cleaning Procedure

## Missing Values

Since we noticed that around 10 companies were founded after Year 2014, we decided to remove those companies from our analysis scope. After running the API script, we have noticed that 10 companies are corrupted and have only returned a few lines of data. Those companies are excluded from the dataset as well, with tickers: AMCR, BF.B, BRK.B, CPRI, GL, LHX, LIN, LW, EVRG, CDW. We believe removing those companies will not have a significant impact on our trend analysis. After we cleaned the data, we have around 10000 observations in our dataset.

## Target Transformation

Since we want to predict the trends of stock price after the release of the financial statement report, we create a target variable which records whether the stock price goes up or down in next quarter after the financial statement was disclosed. We assign true (1) to price going up and false (-1) to going down or staying the same.

# Preliminary Analysis

### Feature Selection

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/correlation1.png" width = "350">


As the first step of our analysis, we selected 7 features from the quarterly financial statement from listing companies based on our knowledge in financial statement analysis, these seven features are: earnings per share(EPS), net profit growth rate(NPGR), book to market(BTM), net profit margin, equity multiplier, return on equity, and, free cash flow to equity. The above seven features covered different aspect
in valuing the stock of companies, including profitability, sustainability, liquidity, leverage level, valuation accuracy, and the healthiness of cash flow. To examine the relationship of these features, we conducted feature correlation analysis and the result is shown below.

From the above correlation matrix, we found that the correlation between EPS and Equity Multiplier is significant, thus we need to consider dropping one of the features or make some adjustment in our further analysis. Apart from this pair, other correlations are not significant.

### Feature Transformation

Considering the facts that different companies have different sizes and companies in different industry sector have different benchmarks in terms of financial statement indicators, we normalized each column in the data: for each column, we subtracted the column mean from each row of data and divided them by column standard deviation. We dropped any rows with NAs and started to fit different classification models.

### Fitting SVM

The first model we used is Support Vector Machine(SVM), as we have little idea about the data, and as our goal is to find a linear separator between &quot;up&quot; stocks and &quot;down&quot; stocks, and SVM works relatively well at finding the linear separator.

Before fitting the SVM, we randomly shuffled our data set and split it into training set and test set according to the &quot;80-20&quot; rule. To ensure the data is not too imbalance, we count the up stocks and down stocks in each set, and the result indicated that the dataset is not too imbalance to fit SVM.

|   | Up | Down | No Change | Up/(Down + No Change) |
| --- | --- | --- | --- | --- |
| Train Set | 4949 | 3219 | 0 | 60.59% |
| Test Set | 1217 | 825 | 0 | 59.59% |

We firstly fit the Linear SVM, and the accuracy of training set and test set is 60.98% and 60.50% correspondingly.

Secondly, we tried SVM with nonlinear kernel and the result is 61.48% and 60.28% for each set. To notice, we took a look at the confusion matrix and the result for auto SVM is shown below.

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/confusion1.png" width = "350">


# Preliminary Reflection

In our preliminary analysis, the training and test accuracy is both around 60% which indicates that our model is significantly underfitting. In this case, we would consider increase the complexity of our model or include more data in our dataset. However, as our goal is to predict the stock trend after the release of quarterly financial statement, and the data that can be extracted is limited to 5 years, which means we are experiencing the lack of enough data, so that we would consider use more complex models, and add more features in our further analysis.

Besides, as shown in the confusing matrix, we were predicting nearly 95% of the stock to be &quot;up&quot; stocks, in our further analysis, we tried to handle this weird problem.

# Model Improvement

We tried different models to fit our data including random forest; logistic regression; Neural Network; AdaBoost and, XGBoost. The train accuracy, test accuracy and the F1 score are summarized below. F1 score, which takes value from 0 to 1,is used to measure the performance of a classification model (reference: [https://en.wikipedia.org/wiki/F1\_score](https://en.wikipedia.org/wiki/F1_score)). The larger the F1 score, the better the model.

|   | Train accuracy  | Test Accuracy | F1 Score |
| --- | --- | --- | --- |
| Linear SVM | 60.98% | 60.48% | 0.7423 |
| Non-Linear SVM | 61.48% | 60.28% | 0.7408 |
| Random Forest | 57.59% | 55.68% | 0.6322 |
| Logistic Regression | 60.97% | 60.38% | 0.7415 |
| Neural Network | 61.84% | 60.28% | 0.7379 |
| Ada Boost | 71.56% | 57.05% | 0.6873 |
| XG Boost | 63.04% | 59.30% | 0.7333 |

Here, we would like to explain the basic concept and mechanism of AdaBoost and XGBoost as they have not been taught in the class. The general idea of boosting algorithm is that the model trains a series of weak models, each trying to correct its predecessor, and the produce a strong model in the end. AdaBoost (Adaptive Boosting) trains a decision tree each time and assign more weight for those incorrectly classified points before training the new tree. XGBoost (eXtreme Gradient Boosting) implements gradient boosted decision trees designed for speed and performance. Gradient Boosting tries to fit the new tree to the residual errors made by the previous tree. XGBoost provides optimization to decrease the computational time since gradient boosting is usually slow.

The graph below shows the change of training accuracy and test accuracy against the number of predictors to be trained in the model. We see that there is an elbow point around 5. This means we only need to train around 5 trees in a boosting model to get the best test accuracy. The accuracy is around 60%.

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/train_test_boost.png" width = "350">

From the model fitting result above, we were surprised to notice that no matter what model we used, our test accuracy was always around 60%. To better understand the underlying reason, we conducted the PCA, and plotted the below graph which explained the underfitting to some extent: the data is not well-separated so that it is hard for us to find a better model which can predict with higher accuracy. Thus, at this stage, we thought of including more features into our model.

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/PCA.png" width = "350">

### One-hot Encoding

As mentioned above, stocks in different industrial sectors may react to financial statement indicators differently, for example, the retail industry may focus on the free cash flow(liquidity) while the manufacturing industry is assumed to be affected largely by the profitability. We divided all companies into 11 sectors, and used one-hot encoding to generate new features for our model.

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/pie_chart.png" width = "350">

Besides, we included more indicators from the financial statement as features. Again, we conducted feature correlation analysis. From the new correlation matrix, we identified that the feature &quot;gp&quot; and &quot;equity&quot; is highly correlated with some other features, thus we decided to drop these two features to prevent overfitting.

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/correlation2.png" width = "350">

The new accuracy comparison table is shown below: the green numbers mean that the accuracy decreases as the increase of feature numbers and the red numbers indicate that the model accuracy was improved.

|   | Train accuracy  | Test Accuracy | F1 Score |
| --- | --- | --- | --- |
| Linear SVM | 60.98% | 60.48% | 0.7423 |
| Non-Linear SVM | 62.12% | 60.43% | 0.7404 |
| Random Forest | 54.44% | 53.18% | 0.5616 |
| Logistic Regression | 61.14% | 60.67% | 0.7435 |
| Neural Network | 62.08% | 60.18% | 0.7341 |
| Ada Boost | 76.27% | 56.26% | 0.6686 |
| XG Boost | 62.95% | 59.94% | 0.7406 |

We concluded that with the new model, logistic regression was improved in terms of both train accuracy and test accuracy, and its test accuracy was the highest among all models, i.e. 60.67%. Based on that, we decided to dig deeper using logistic regression model to find optimal parameters.

We fitted two logistic regression model with L1 and L2 regularizer respectively. We created two plots to see the change of accuracy:

#### L1 regression:

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/l1.png" width = "350">


#### L2 regression:

<img src= "https://github.com/RyChen-Cornell/ORIE4741-Group-Project/blob/master/final%20report%20img/l2.png" width = "350">

We see that the best train and test accuracy are still around 60%. The L1 regression has better test accuracy.

# Discussion on Weapons of Math Destruction and Fairness

We believe our model will not suffer the problem of Fairness because we use the financial statements of each company which are observed from the performance of the company. For WMD, we think our model is measurable because one can observe the movement of stock prices in financial markets to check whether our model performs well or not. Our model will not have negative consequences because after all, the price movement is determined by the whole markets and other different factors, and we are sure that our model does not have the power to change the financial markets. It is because of this same reason that our model will not create self-fulling feedback loops.

# Conclusion and Future Work

We are upset that we cannot find a model that outperforms other models significantly. The best model we have found concludes a ~61% of test accuracy.

However, if we consider the problem from the perspective of investment and finance, it is not surprising at all. There are two major investment philosophies which are popular in the market: passive investment and active investment. The difference between these two strategies is basically whether fund managers would actively form and rebalance their portfolios, but there is no clear evidence active investment would outperform passive investment. Thus, if we can predict the stock movement trend with a 60% accuracy, we have already been able to beat most of the funds on the street. We have also read several similar academic papers trying to predict the stock movement trend, and none of them could get an accuracy rate that is higher than 65% even with deep learning models. However, to implement 60%-accuracy model, there are still lots of problems, such as extreme events, trading limitations and liquidity issues.

Thus, there is much further work to be done to make a more accurate and feasible prediction of stock movement trends.

1. Take trading constrains and other realistic issues into consideration.
2. Increase the size of the dataset by either shorten the time gap of each recording date or increase the length of whole time period
3. Include more features from technical analysis. In our model, we focus basically indicators from fundamental analysis. However, as believers of market inefficiency, we think technical analysis would make a difference, thus we could include some technical indicators like RSI, MACD in our model.
4. There is an interesting factor that we want to mention: in the correlation matrix of our final model, unlike other sectors, the financial industry sector seems to have higher correlations with some of the features, so we are interested in narrowing our analysis scope to financial industry only, and we are optimistic to find a specific model for financial industry with higher accuracy.

# References

1. [https://medium.com/greyatom/a-quick-guide-to-boosting-in-ml-acf7c1585cb5](https://medium.com/greyatom/a-quick-guide-to-boosting-in-ml-acf7c1585cb5)
2. [https://escholarship.org/uc/item/0cp1x8th](https://escholarship.org/uc/item/0cp1x8th)
