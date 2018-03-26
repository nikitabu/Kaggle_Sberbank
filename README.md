# Kaggle: Sberbank's Russian House Price Prediction Competition

Sberbank's 2017 Kaggle Competition asks to predict price fluctuations in the Moscow housing market.

Specifically, Sberbank asks us to predict the RMSLE (Root Mean Squared Logarithmic Error) between predicted prices and actual data. The provided data set consists of about 30,000 transactions, with almost 300 features (e.g. square footage, number of floors, vicinity to schools, workplaces, factories, etc.), as well as macroeconomic information such as currency exchange rates, income per capita, and average rental values.

An exploratory data analysis I performed (link below) revealed numerous pieces of very important information. 
An inspection of the correlation matrix showed that there were several blocks of redundant (non-orthogonal) features. I merged such features together, to reduce the dimensionality of the space without substantial loss of information.

Likewise, an inspection of the macroeconomic data showed that very few of those features were potentially relevant to the task of predicting housing prices.

Examining the distribution of housing prices, I found that although the majority of prices were in a roughly log(Gaussian) distribution, there were prominent spikes at 1, 2, and 3 million rubles, which did not look at all natural. The reasons for this were unclear, but they have been the result of erroneous or temporary data entries. A deeper inspection into other features revealed a clear data quality problem. There were numerous missing values, inconsistencies between features, and examples of either errors or outright fraud in the market. 

My initial approach involved various models (StatsModels ridge regression, SKLearn random forest, XGBoost) with basic preprocessing steps.

I replaced obvious outliers in the feature data with their corresponding median values, dropped features with very large numbers of missing values, and filled in the remaining missing values with the corresponding median or modal values. Text-based features were converted to ordinal features or non-ordinal dummy variables as need. Variables with extremely low levels of correlation with the price were removed. Highly skewed variables were log-normalized. 

Examination of predicted vs. measured plots, and the residual distribution plots, showed that the 1M, 2M, 3M prices were causing pronounced errors. To deal with this issue, I tried several approaches. In the first approach, I dropped the problematic prices, and replaced their values using those determined by the MICE algorithm. In a second approach, I separated the problematic data points into a separate data set, trained an initial classifier on the clean data, predicted the values of the "dirty" data set, and then retrained a classifier on the new data set. 

This approach, in combination with a merging of several other XGBoost models, was enough to get me in 302/3274, a bronze model. This was my first Kaggle competition that I put substantial time into, and I'm somewhat satisfied with my performance. I learned a lot about how to deal with data quality problems in the Kaggle discussion groups. There were some very valuable pieces of code Kaggle users shared that were crucial to getting the scores I did. And I can't say I would've gotten anywhere near the score that I did on my own.
