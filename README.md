# Business Understanding

The forecasting of future natural gas consumption patterns is essential for energy planning and ensuring a reliable supply. Time series models, such as ARMA and SARIMA, have been shown to effectively capture the seasonality and growth trends in natural gas usage, providing valuable insights for decision-makers. Additionally, accurate forecasts help in managing the economic aspects of natural gas production and consumption, enabling industries to operate more efficiently and reduce economic risks. Understanding consumption patterns through time series analysis supports the development of sustainable energy policies and can guide investment in infrastructure and technology to meet future energy demands. Time series modeling is a vital tool for organizing an analysis base for natural gas inspection and promoting effective and credible energy management in the United States. The stakeholder for this project is North American Electric Reliability Corporation (NERC) and by forecasting natural gas consumption for the next three years, I will be able to provide recommendations for utility and gas companies.

Source: https://www.eia.gov/electricity/data/browser/#/topic/2?agg=2,0,1&fuel=f&geo=g&sec=g&linechart=~ELEC.CONS_TOT.NG-US-99.M&columnchart=ELEC.CONS_TOT.COW-US-99.M&map=ELEC.CONS_TOT.COW-US-99.M&freq=M&start=200101&end=202312&ctype=linechart&ltype=pin&rtype=s&maptype=0&rse=0&pin=

![coverphoto](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/coverphoto.jpg)


# Data Understanding

For this analysis I collected data from the United States Energy Information Administration and within the dataset there are the monthly consumption totals of coal, petroleum liquids, petroleum coke and natural gas for all sectors. This dataset has every month’s usage since January 1, 2001, and ends on December 1, 2023.

![fire](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/fire.JPG)


## Data Preparation

This dataset is in the form of a csv and currently the datatypes are all strings. First I will need to drop rows and columns that do not contain any data, then I will change the index to date and transpose the dataset so that each date will have it's corresponding natural gas comsumption amount. Finally, since for this analysis I am only focusing on Natural Gas, I will drop all columns other than "Natural Gas" and get the descriptive statistics on the new dataset.

![originaldataset](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/originaldataset.JPG)


# Exploratory Data Analysis

For this section, I wanted to show what the dataset looked like all together on a line graph, so that we can clearly see the trend. It was also important to split the dataset in two, train and valid, so that we can train the dataset on one group and then test the model on unseen data. The training set has 80% of the data and the test set has 20% of the data.


![originalplot](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/originalplot.JPG)

![traintestsplit](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/traintestsplit.JPG)


# Modeling

I created several models for this analysis, starting with a simple naive baseline model, then have two versions of each Autoregrssive, Moving Average and ARMA models. Finally, the dataset was run through a SARIMAX model that takes into account the seasonality of the data. My goal was to improve the results through each interation and end with the best performing model. The metrics that are used for success in time series analysis problems is RMSE (Root Mean Squared Error) and AIC (Akaike information criterion.) For this analysis, the smaller the score, the better.

## Baseline Model

First I built the baseline naive model, where the datset is shifted by 1 and is now equal to the day before. The RMSE value for this model will be what I try to improve upon and compare against when I create the other models to follow.

![shiftedmodel](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/shiftedmodel.JPG)



The RMSE value for the baseline is one of the values I will be comparing the models by. RMSE equals the difference between the predicted value and the true value in the original dataset. This basically shows us how close our model is to predicting the correct value, so the smaller the value, the better but it is relative to the values in the dataset. The RMSE value is quite high, so with creating models with more complexity I think that number will be lowered significantly.

![residuals](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/residuals.JPG)

![decomp](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/decmp.JPG)

We can see from this decomposition chart that there is an upward trend, which indicates a general overall growth in the production of natural gas. We can also see a seasonality, with lags of 1 year, with high peaks during the summer months. We can't really see a pattern in the residulas, which is what we want.



## Autoregressive Model (AR)

In this section, I will begin working on ARMA models, Autoregressive Moving Average models, which are used to describe weakly stationary stochastic processes. They combine two components:

Autoregressive (AR): This part models the variable as a linear combination of its own previous values. The AR part is denoted as AR(p), where ( p ) represents the number of lag terms used in the model.

Moving Average (MA): This part models the variable by using the past forecast errors in a regression-like model. It is denoted as MA(q), where ( q ) represents the number of lagged forecast errors in the prediction equation.

I will first start with two AR models, then move on to two MA models. Finally, I will use ACF and PACF charts to determine the best parameters for the ARMA model.

![differenced](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/differenced.JPG)

![ar1](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/ar1.JPG)

This was a litte more complex of a baseline model. I wanted to use 1 in the parameters just to see what the AIC and RMSE values would be and how they could improve with tuning of the model later one. This did not perform terribly but I think it can be improved.

![ar2](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/ar2.JPG)

Replacing the 1 with a 2 did not improve the model's performance by much at all but it is moving in the right direction. Next I will move on to the Moving Average Model and see how the model performs by changing the 'q' parameter.


## Moving Average Model (MA)

![ma1](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/ma1.JPG)

The improvement by changing the q parameter decreased the aic and rmse score by a much larger margin than just changing the 'p' parameter from 1 to 2. Again, we are moving in the righ direction but I think the scores can still be improved.

![ma2](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/ma2.JPG)

Since I changed the 'p' parameter from 1 to 2 in the AR model, I wanted to do the same thing for the MA model. Again, we are seeing an improvement in the model performance but a RMSE value of 102586 is not a score that would be sufficient in accurately forecasting natural gas consumption. Putting together the AR and the MA model for a ARMA model should increase th performance and then I will continue to hypertune the parameters.


## ARMA Model

Since both the AR 2 and the MA 2 model performed better than the first, I will start the ARMA model with 2 in each p and q parameters. Then, I will use ACF and PACF to get the optimal parameters for the model.

![darma1](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/arma1.JPG)

This is the largest improvement we've seen so far! The AIC score lowered by about 40 but the RMSE score lowered by about 14,000. Instead of hoping the parameters I set improve the model, next I will create ACF and PACF charts to get the exact parameters needed for optimal model performance.

![ACF](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/ACF.JPG)

According to the ACF chart, the best value for 'p' is is 13.

![PACF](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/PACF.JPG)

The PACF shows that both 9 and 12 would be best for the 'q' position in the ARMA model.

![arma2](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/arma2.JPG)

Not surprisingly, this is the best performing model we've seen so far. Adding 13 and 9 in the p and q slots significantly improved the models performance. The AIC score dropped by about 600 and the RMSE decreased by over 30,000! By taking the range value and RMSE score, I calculated the Root Mean Squared Percentage Error, which is approximately 8%.

![arma2_residuals](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/arma2_residuals.JPG)

![arma2forecast](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/arma2forecast.JPG)


## SARIMAX Model

A SARIMAX model, which stands for Seasonal Autoregressive Integrated Moving Average with Exogenous Regressors, is an extension of the ARIMA model. It is designed to handle time series data that not only have a trend and seasonality but are also influenced by external variables.

Seasonal (S): Captures the seasonality in the data, which are patterns that repeat over regular intervals, like daily, weekly, or yearly cycles.

Autoregressive (AR): Models the relationship between an observation and a specified number of lagged observations. Integrated (I): Involves differencing the data to make it stationary, which means removing trends and seasonality from the data.

Moving Average (MA): Models the relationship between an observation and a residual error from a moving average model applied to lagged observations.

Exogenous Regressors (X): Allows the model to include external variables that could affect the time series but are not part of the series itself.

SARIMAX models are particularly useful for forecasting when data show patterns that repeat over time and when such patterns are influenced by external factors

![sarimax](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/sarimax.JPG)

After doing a grid search to find the best parameters for lowest AIC score, I was surprised to see such a high RMSE score. Initially I was using AIC as the main metric for model performance but after seeing this RMSE score, I feel that it is more important for our business problem to have a more accurate model than the best fitting model. So I will use this model on the validation dataset but will probably choose the ARMA_2 model as the best performing model.

![sarimaxpred](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/sarimaxpred.JPG)

![sarimaxzoom](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/sarimaxzoom.JPG)

![sarimaxforecast](https://github.com/lpb3393/natural_gas_consumption/blob/main/photos/sarimaxforecast.JPG)

The model performed better on the entire dataset but since the AIC score and the RMSE score are still significantly higher than those of the ARMA 2 model, that is still the best performing model for this analysis.



# Evaluation

The evaluation of these models for natural gas consumption indicates that the ARMA model outperforms the SARIMAX model. This conclusion is drawn from the comparison of the Akaike Information Criterion (AIC) and the Root Mean Square Error (RMSE) scores for both models. The ARMA model exhibits an AIC score of 6829.150, which, despite being higher than the SARIMAX’s AIC score of 376.28, suggests a better fit to the data when considering the context of the scores. Additionally, the ARMA model’s RMSPE score of 4.74% (RMSE of 56295) is significantly lower than the SARIMAX’s RMSPE of 9.13% (RMSE of 108275.2), indicating a more accurate prediction of natural gas consumption with less deviation from the observed values. These metrics collectively suggest that the ARMA model, with its lower complexity and better predictive accuracy, is more suitable for forecasting natural gas consumption in this scenario.

The ARMA model’s better performance is also reflected in its ability to capture the dynamics of natural gas consumption without the need for the additional seasonal components that SARIMAX incorporates. While the SARIMAX model’s lower AIC score might suggest a better fit, the significantly higher RMSE points to less accurate predictions, which is critical for practical applications. When I started this analysis, I was using AIC as the main deciding factor for model performance. When I got the SARIMAX models, I decided that because of our business problem having a lower RMSE score was more important. A low AIC score, while is important, isn't the best indicator for model performance accuracy. In predictive modeling, especially for time series data, the goal is to achieve a balance between model complexity and predictive power.


## Limitations

The ARMA model, while useful for time series forecasting, has certain limitations when predicting natural gas consumption. One of the primary constraints is its assumption of linearity and stationarity in the time series data. Natural gas consumption can be influenced by a range of non-linear factors such as economic cycles, policy changes, technological advancements, and unpredictable events, which the ARMA model may not fully capture. Additionally, the model does not inherently account for seasonal patterns, which are often present in energy consumption data. This can lead to less accurate forecasts, particularly over longer time periods or during periods of significant seasonal variation.

Another limitation is the ARMA model’s sensitivity to the choice of parameters and the requirement for pre-processing steps like differencing to achieve stationarity. The model’s performance heavily depends on the correct identification of the order of the autoregressive (AR) and moving average (MA) components. Misidentification can result in overfitting or underfitting, leading to poor predictive performance. The ARMA model does not accommodate exogenous variables or interventions that could impact natural gas consumption, such as changes in prices or regulatory policies. This lack of flexibility can make the ARMA model less robust compared to models that can incorporate such external factors, like the SARIMAX model. Run time is also a big limitation when performing grid searches for ARMA and SARIMAX models. Grid searches allow us to choose the most optimal parameters for the model but each time the code block was changed, it took over 12 hours to see the results. The could result in using smaller parameters or not using the best parameters for performance if short on time.

## Recommendations


In light of modeling that shows increased demand for natural gas, here are three recommendations for gas and utility companies:

Invest in Infrastructure: To meet the rising demand, companies should consider investing in the expansion and modernization of their distribution and transmission infrastructure. This includes developing new pipelines, storage facilities, and more terminals to ensure reliable delivery and storage of natural gas.

Diversify Supply Sources: Companies should diversify their natural gas sources to mitigate risks associated with over-reliance on a single supplier or region. This could involve securing contracts with multiple suppliers, exploring alternative sources and investing in renewable energy to complement natural gas usage.

Enhance Energy Efficiency Programs: Implementing and promoting energy efficiency programs can help manage demand and reduce the overall consumption of natural gas. Gas utilities can offer incentives for customers to adopt energy-efficient appliances, improve home insulation, and implement smart energy management systems to optimize natural gas use.




## Next Steps

To improve my model in the future, these steps can be taken to give a more complete and well rounded approach to solving this problem:

Include renewable engergy sources in the analysis to see how they compare to natural gas and how they might affect each other.
Go more indepth to the seasonal variations in the data and how that has affected the performance of this model.
Perform this analysis on a much larger dataset and one that isn't averaged every month.



## For More Information
See the full analysis in the [Jupyter Notebook](https://github.com/lpb3393/natural_gas_consumption/blob/main/natural_gas_consumption.ipynb) or review the [presentation](https://github.com/lpb3393/natural_gas_consumption/blob/main/natural_gas_consumption_presentation.pdf)




```
├── data
├── photos
├── .gitignore
├── README.md
├── natural_gas_consumption_presentation.pdf
└── natural_gas_consumption.ipynb
```

## For Reproducing Analysis
You can recreate the analysis in the [Google Colaboratory](https://colab.research.google.com/drive/15LbgfiI9YhifuU_VM9oXnEMq37Nl_MVL#scrollTo=GlTeqzx5zeHy). 
The data is pre-loaded in the file but can also be downloaded directly from the [EIA Website](https://www.eia.gov/electricity/data/browser/#/topic/2?agg=2,0,1&fuel=f&geo=g&sec=g&linechart=~ELEC.CONS_TOT.NG-US-99.M&columnchart=ELEC.CONS_TOT.COW-US-99.M&map=ELEC.CONS_TOT.COW-US-99.M&freq=M&start=200101&end=202312&ctype=linechart&ltype=pin&rtype=s&maptype=0&rse=0&pin=). To download the dataset, click the link provided and select "Download" on the page. You can then select the format needed. 