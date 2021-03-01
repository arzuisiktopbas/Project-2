# Algo Trading
 *by  Patrick DeStefano, Freddie Eisa, Terry Fry, Hope Forrester, Arzu Isik Topbas*
 
 

## Hypothesis
In this project, we created an algo trading model for buy, sell, hold prediction,  using FinViz to identify key stocks given input from current news on positive or negative sentiment in order to maximize returns.


## Exploration and cleanup process

We pulled stock tickers from full articles using the newsapi. After conducting sentiment analysis, we dropped tickers with negative or neutral sentiments and kept only top 25 tickers with positive sentiment scores over 0.2.

We utilized Alpaca to pull stock data on the list of 25 previously obtained. This data was cleaned and pushed through FinViz’s screener feature to determine which stocks were optimal for the machine learning model based on the following criteria:

 * Performance : Today +10% 
 * Current Volume : Over 10M
 * Country : USA
 
We had issues with Alpaca being down which made it impossible to run and test our code, but luckily it returned. This forced us to consider using other data sources like Pandas Data Reader or Yahoo Finance.

**ARIMA**

After receiving the price predictions, we found code that would have allowed us to create a dynamic & functional paper trade using the alpaca api. 
The most difficulties arose from comparison of our predicted prices to original real-time prices. Putting them into a DataFrame in order to assess percentage up/down proved time consuming.

**LSTM**

We pulled data from Yahoo API due to difficulties with Alpaca api not supporting adjusted close.
Split ratio changed the mode significantly. The more we increase the split ratio, the more accurate our model became, however, the higher we increased our split ratio, the less data we had for backtesting.


## Models

**VADER**

In order to begin our algo model, we decided to first look for stocks with a positive sentiment in the news.
Vader provided us with a sentiment score, with which we were able to decide on the top 25 stocks with the most positive sentiment. 
This yielded a comprehensive list of stocks to evaluate further using FinViz. 
Vader was our first choice for preliminary stock choice to gather a larger list of stocks with the propensity for higher returns based on a positive sentiment score. 

**ARIMA**

We chose ARIMA as our algo-trading model as it was the simplest model with the lowest P-Value at 0.00
We attempted Logistic Regression, Linear Regression, GARCH, ARMA, and ARIMA to compare all models to see which would provide us the best results. Out of all models, ARIMA provided the consistent time-series predictions whereas the others varied in consistency and GARCH would have been a good “add-on”, but not a good model in it’s own in order to make time series predictions of buy, sell, hold. 
We discarded this model in the end as it was not highly predictive and presented evaluation issues.

**LSTM**

We moved to LSTM for our algo trading model as it was simple but highly predictive and gave us more features to adjust to get a better predictive model. This proved easier to quickly evaluate than ARIMA. 

## Model Evaluation

**ARIMA Model Performance**

AR and MA terms have p values of zero, which suggests that they are useful to predict prices.
We tried AR and MA up to 5 lags, 1 and 2 for integration term. Arima (1,1,1) has the lowest AIC & BIC.

**LSTM Model Performance**
Our first LSTM model gave  19% loss  so to adjust it we adjusted:

    * split down 0.1 to 0.7
    * dropout layer down 0.5 to 0.2 
    * Resulting in lower 18% loss
    
![image source](https://github.com/arzuisiktopbas/Project-2/blob/main/Images/Prediction_%2518.png)
![image source](https://github.com/arzuisiktopbas/Project-2/blob/main/Images/Prediction_%2519.png)


## Model Summary

Our Vader model returned a selection of tickers with positive sentiments that allowed us to narrow our search for tickers worth trading.

Our ARIMA model gave us price predictions that would have allowed us to make buy/sell/hold decisions per stock, but not as dynamically as we would have liked. 

Our initial  LSTM model resulted in 19% loss and as we adjusted we obtained a lower loss at 18% thus showing lower split, lower layer yielded better results. 

We evaluated our model by Mean Absolute Error that computes the mean absolute error between the labels and predictions. For our 18% loss model, we obtained a 0.35 Mean Absolute Error. This is fairly low which we want to see! 

