# stock-direction-prediction

## Summary
This repository contains code to train and run a model used to predict the direction a stock from a particular list of large tech 
companies will move over the next week and over the next quarter. The model first smoothes the data for each company by replacing the
datapoints with moving averages over a user-defined amount of days (set by the variable MEAN_LENGTH). The problem is then converted into 
a supervised learning problem by using lag variables derived from the smoothed data. The lag variables will skip a user-defined number of
days between each feature (set by the variable JUMP) and will go back a user-defined number of days (set by the variable FEATURES). The 
data is then scaled and the dimensionality of the data is reduced via PCA. Then, a SVM is trained on this transformed data. 80% of the data
(4 years worth of data) was used for training and the last 20% (1 years worth) was used for testing. Additionally, there is the option to
use a Gradient Boosting Classifier.

## Model Assessment
The model was tested both for predictions 1 week ahead and for its predictions 1 quarter ahead. In both cases, both the models precisions
at predicting upward movements and its precisions at predicting downward movements were assessed. The model was compared to three different
models in each of these cases: a random model, a model which always predicts that the stock moves in the direction in question and a model
which predicts according to the trend of the smoothed data. To test our model against each of these models, all of the models were tested
on their prediction in each year for the past 5 years. For our model, for each new test set we retrained it on the four years prior to the
test set. The relevant precisions were then calculated for each model and stored in a Numpy array after which a non-parametric F-test was 
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
