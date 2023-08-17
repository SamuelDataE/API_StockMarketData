  ## General Information about Alpha Vantage
 Alpha Vantage is a financial data provider and API platform that offers real-time and historical financial market and stock data. It allows developers to access stock prices, forex rates, cryptocurrency data, and technical indicators through a programming interface. Alpha Vantage also provides tools for analyzing and visualizing financial data, used by investors, developers, and researchers to gain insights into the markets and develop trading strategies.

 The big advantage over other data providers is that Alpha Vantage provides most of the functions free of charge. Therefore, in these use cases, we will download the data from Alpha Vantage.

## Setup - API Key
To download data from Alpha Vantage, you need a personal API key. You can request this at [Alpha Vantage Support](https://www.alphavantage.co/support/#api-key). Once you have entered your details, a code will be generated below. It is important that you save this code with you.

![Request API Key](/images/AV_APIKEY.png)

## What data to download
From Alpha Vantage, you can download data such as real-time stock prices, historical price data, forex rates, cryptocurrency data, and technical indicators, which can be used for financial market analysis and trading strategy development.

The procedure for querying the necessary data is detailed in Alpha Vantage's [documentation](https://www.alphavantage.co/documentation/). In our scenario, we intend to download daily stock market data for various stocks. Hence, we utilize the Quote Endpoint for each respective stock. The code marked below illustrates the implementation.

![Request Code](/images/AV_Documentation.png)

This code is used in all three approaches.

