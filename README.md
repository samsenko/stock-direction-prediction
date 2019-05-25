# stock-direction-prediction

## Summary
This repository contains code to train and run a model used to predict the direction a stock from a particular list of large tech 
companies will move over the next 7 business days. Firstly, Singular Spectrum Analysis (SSA) is run on the data to transform it into a trend component, multiple cyclic components and multiple noise components. The model is then trained using the components produced by SSA as features. Both a Gaussian Naive Bayes model and linear Support Vector Machine model are implemented. Non-linear SVMs were tried but did not yield as high results. The model was trained on the first 80% of the data and tested on the last 20%. 

## Model Assessment
The models were tested on their prediction for a specific stock using a variation of KFold cross-validation. Specifically, the dataset was split into 21 consecutive segments and, for each i from 1 to 20, the model was trained on the first i segments and tested on the (i+1)st segment. We ignored the results of tests where the model was trained on less than 5 years worth of data as these tests likely used insufficient data to effectively train the model. As an example of the results of these tests, for predicting the direction of Facebook's stock, the mean accuracy of the Naive Bayes model had a 95% confidence interval of 74% +/- 5% and the SVM had a 95% confidence interval of 77% +/- 3%.

We also performed statistical hypothesis tests (specifically a paired student's T-Test) based off of these results comparing the two models and comparing the models to a simple baseline model which uses only the trend in the closes over the past week to predict the direction. Again using the example of Facebook, we found no significant difference between the Naive Bayes and SVM models (p = 0.22). However, we did find highly significant evidence that both of these models outperformed the baseline model (p < 0.001).

The results for the other stocks were mostly similar (with our models having mean accuracies in the 70-80% range and showing significant evidence of outperforming the baseline model). Notably, however, the Naive Bayes model underperformed on the BB data (with a mean accuracy of only 60%). On the other hand, the SVM model still managed to perform very well on this data. The difference in performance for the Naive Bayes model can likely be explained by the fact that the BlackBerry stock has quite a different history compared to the other stocks.

## Files in this Repository
This repository contains a Jupyter Notebook called Stock_Direction_Prediction.ipynb which contains the code for feature engineering, model 
training, model assessment and a function called predict_direction which allows you to predict the direction of a stock's movement given the historical data. 

This repository also contains ten year data for each of the companies we were trying to predict. The companies are all major tech companies (Apple, Google, Facebook, Blackberry, Microsoft, IBM and Samsung). Data was collected from Yahoo Finance.

## Future Work
In the future, one might attempt to incorporate this model into an algorithmic trading model either directly or, more likely, combining it with other models in quantitative finance.

## Addendum
There are a couple of caveats to these results. Firstly, financial data is extremely variable and therefore it is inevitable that a model will overfit to the available data. Additionally, to efficiently use SSA, it was necessary to fit_transform the train and test data together. This could introduce some look ahead bias. The predict_direction function does not have this problem (and is quite inneficient as a consequence). The extent to which the results are overstated can be estimated by using the predict_direction function on completely new data from after the model was built (there is very little of this) and on the original test data (which may overfit but at least removes look-ahead bias). Both of these tests suggest that the actual accuracies are just under 60%. This is significantly lower than the original validation results; however, it is worth noting that a mean accuracy of 60% is still a statistically significant improvement over trend-based trading, being, for example, above the 90% confidence interval on the trend scores for the Facebook data and above the 95% confidence interval on the trend scores for the Microsoft data.
