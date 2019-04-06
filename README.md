# stock-direction-prediction

## Summary
This repository contains code to train and run a model used to predict the direction a stock from a particular list of large tech 
companies will move over the next week and over the next quarter. The model first smooths the data for each company by replacing the
datapoints with moving averages over a user-defined amount of days (set by the variable MEAN_LENGTH). The problem is then converted into 
a supervised learning problem by using lag variables derived from the smoothed data. The lag variables will skip a user-defined number of
days between each feature (set by the variable JUMP) and will go back a user-defined number of days (set by the variable FEATURES). The 
data is then scaled and the dimensionality of the data is reduced via PCA. Then, a SVM is trained on this transformed data. 80% of the data
(4 years worth of data) was used for training and the last 20% (1 years worth) was used for testing. Additionally, there is the option to
use a Gradient Boosting Classifier.

## Model Assessment
The model was tested both for predictions 1 week ahead and for its predictions 1 quarter ahead. In both cases, both the model's precisions
at predicting upward movements and its precisions at predicting downward movements were assessed. The model was compared to three different
models in each of these cases: a random model, a model which always predicts that the stock moves in the direction in question and a model
which predicts according to the trend of the smoothed data. To test our model against each of these models, all of the models were tested
on their predictions on each year for the past 5 years. For our model, for each year we retrained it on the four years prior to that
year. The relevant precisions were then calculated for each model and stored in a Numpy array after which a non-parametric F-test was 
used to compare the models.

This method has a couple of advantages. Firstly, by using a statistical hypothesis test, we can not only see which model is more accurate
but also can gain a sense of the significance of these differences in accuracy. Additionally, using seperate years for test data attempts
to increase the independence of the precisions used, an assumption used by most hypothesis tests (unfortunately, doing so also means that
our sample size is much smaller which may result in p-values being overstated). Lastly, using a non-parametric test limits the assumptions
we are making about the distribution of the precisions.

Based off of these tests, we obtained the following results. Although our model outperformed all other models in all situations, we found
no statistical significance to this result in the following scenarios:

1. Precisions on upward predictions of our model predicting 1 quarter ahead against a model predicting always up (p = 0.5)
2. Precisions on upward predictions of our model predicting 1 quarter ahead against a model predicting based on the trend (p = 0.7)
3. Precisions on downward predictions of our model predicting 1 quarter ahead against a model predicting based on the trend (p = 1.0)
4. Precisions on upward predictions of our model predicting 1 week ahead against a model predicting always up (p = 0.5)

We did, however, find some statistical evidence that our model was consistently outperforming other models in some situations, especially
when considering that the p-values may be inflated due to the very small samples size of N = 6. These are the following scenarios:

1. Precisions on upward predictions of our model predicting 1 quarter ahead against a model predicting randomly (p < 0.01)
2. Precisions on downward predictions of our model predicting 1 quarter ahead against a model predicting randomly (p = 0.05)
3. Precisions on downward predictions of our model predicting 1 quarter ahead against a model predicting always down (p = 0.03)
4. Precisions on upward predictions of our model predicting 1 week ahead against a model predicting randomly (p < 0.01)
5. Precisions on upward predictions of our model predicting 1 week ahead against a model predicting based on trend (p = 0.11)
6. Precisions on downward predictions of our model predicting 1 week ahead against a model predicting randomly (p < 0.01)
7. Precisions on downward predictions of our model predicting 1 week ahead against a model predicting always down (p < 0.01)
8. Precisions on downward predictions of our model predicting 1 week ahead against a model predicting based on trend (p = 0.13)

Based on this, I conclude that the model is clearly better than randomly guessing both at predicting upward and downward movement in 
stocks and across various time periods (including over only 1 week). Additionally, the model shows clear evidence of consistently 
outperforming slightly more sophisticated methods based on trend in moving average when predicting movements over short time periods (ie. 
1 week). Finally, it is not yet clear whether the model would consistently outperform these slightly more sophisticated models when 
predicting movements over longer time periods.

## Files in this Repository
This repository contains a Jupyter Notebook called Stock_Direction_Prediction.ipynb which contains the code for feature engineering, model 
training, model assessment and a function called predict which allows you to predict the direction of a stock's movement. 

This repository also contains both five year data and ten year data (used only for assessment) for each of the companies we were trying to
predict. The companies are all major tech companies (Apple, Google, Facebook, Blackberry, Microsoft, IBM and Samsung). Blackberry in 
particular was included to increase the diversity of the data by adding a company whose stock price has not consistently increased over
the past ten years. Data was collected from Yahoo Finance.

## Future Work
The model could likely be improved by further tuning of the many hyperparameters (FEATURES, JUMP, MEAN_LENGTH, N_COMPONENTS, C and gamma).
Additionally, adding more features than just lag variable derived from the moving average-smoothed data may improve the accuracy of the 
model. Finally, increasing the amount of data both so as to increase the sample size on the significance tests and to allow for the
prediction of more companies would likely be beneficial.
