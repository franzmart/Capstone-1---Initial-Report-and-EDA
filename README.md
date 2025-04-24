## Using Machine learning models to forecast electricity prices in Houston, Texas. 

**Franz Martinez**

### Executive summary

**Project overview and goals:** The goal of this project is to develop a model to forecast the day ahead electricity price in Houston Texas. We will obtain the model data from Electric Reliability Council of Texas (ERCOT), which is a USA government agency accountable to conciliate the power demand and supply in Texas. Additionally, we will train and test three different machine learning models to forecast the day ahead electricity price, and evaluate the models based on Root-mean-square deviation (RMSE). Lastly, we will select the machine learning model with lower RMSE.

**Exploratory Data Analysis**
Note: ALL the diagrams are in the Jupyter file with similar comments.

We obtained electricity price, electricity demand, wind and solar energy provision datasets for 2022, 2023, and 2024 years from ERCOT website, and filtered the data to retain information for Houston, Texas. All the datasets were consolidated in one single electricity dataset organized by dates and hours. 

The dataset structure was inspected, and we observed that we had to transform the price and hour fields to numeric data type. Similarly, the date field had to be transform to a date data type.

As for cleansing, we inspected the full electricity dataset and found no empty records for any of the fields in the dataset.

Before starting the outlier cleansing, we decided to analyze the Houston electricity price behavior per year. For such purpose, we created four new date fields: Day, Month, Year, and DayandMonth. Once the information was ready, we created a line plot for day ahead electricity prices per month for 2022, 2023, and 2024. From the plot, price data showed peaks demonstrating a clear seasonality change for each year. These seasonal changes were due to weather factors and the heat waves leading to increased electricity demand and as a result prices. We need more granular analysis to define the right features for this model.

As part of our analysis of price behavior, we created a line plot for day ahead electricity prices per hour for four months (March, April, June, and July) of the training data for 2022, 2023, and 2024. From the plot, we observed that the price was doubled peaked in the evenings at 4:00 p.m. and 7:00 p.m. The forecast model is required to capture these effects. The price is clearly hour dependent. We therefore must use the built-in Hour feature. 

We can further refine the above analysis by drilling down to days of the week. We prepared a plot to display the average hourly price for weekday and weekends, computed for 4-month training data. From the diagram, the pattern of prices is almost the same in terms of peak hours for weekdays and weekends. However, the weekdays have different levels of price data in the peak area.

These observations lead to the following feature engineering configuration of our Machine Learning Price Forecast model:

1. Weekday and Week Number as built-in features or predictors to the AI Model to indicate the days of the week.
2. Hour and Is Working Day built-in to reflect other patterns within a day.
3. Year as a built-in feature or predictor to the ML Model to indicate the year.

In a nutshell, our model will have the following variables.

Lagged Variables - Previous Price (which is the previous hour electricity price)
Built-in Predictors - Hour, Weekday (with dummies), Week Number, Month, Is Working Day.
External Time Series Predictors - Demand, Wind supply, and Solar supply

As for removing outliers, we prepared a electricity price histogram, and observed prices beyond 100 dollars. Those prices were uncommon, and therefore, we removed them as outliers. No electricity price can go beyond 100 dollars. We apply this filter for the Electricity Price and Previous Price fields. Additionally, we applied the log function to the prices to smooth them.

Lastly, we set our target variable as Electricity Price and started our correlation analysis and t test for each predictor variables. We observed the following variables as significant predictors.

1. Hour
2. Previous_Price
3. Week_number  
4. Year
5. Month
6. Is_Working_day
7. Monday (Dummy)
8. Friday (Dummy)
9. Saturday (Dummy)
10. Sunday (Dummy)

Now that we have our autoregressive model with the significant predictors, we move on with the models. 

**One ML algorithm**
We prepared the train and test datasets from the electricity dataset. After that,  we calibrated a Ramdon Forest Model with GridSearchCV and applied to the train dataset. 

The optimal parameters for the model were: 'max_depth': None, 'min_samples_leaf': 2, and 'min_samples_split': 5. 

The Ramdon Forest Model has a RMSE value of 0.07 on the test dataset.

Lastly, we conducted a simple test. We tried to predict the ERCOT day ahead electricity price for last Friday (04/18/2025). The actual price at noon was US$ 28.58, and the model price forecast was US$ 31.23. The difference is around 3 dollar, which is reasonable.

### Contact and Further Information

Franz Martinez

Email: fmbowarte@gmail.com 

[LinkedIn](https://www.linkedin.com/in/franz-martinez-auditor/)


