# stock-direction-prediction

## Summary
This repository contains code to train and run a model used to predict the direction a stock from a particular list of large tech 
companies will move over the next 7 business days. Firstly, Singular Spectrum Analysis (SSA) is run on the data to transform it into a trend component, multiple cyclic components and multiple noise components. The model is then trained using the components produced by SSA as features. Both a Gaussian Naive Bayes model and linear Support Vector Machine model are implemented. Non-linear SVMs were tried but did not yield as high results. The model was trained on the first 80% of the data, with the last 20% being held aside for testing. 

## Model Assessment
The models were tested on their prediction for a specific stock using KFold cross-validation. Specifically, the dataset was split into 20 consecutive segments and, for each i from 1 to 20, the model was trained on the first i segments and tested on the (i+1)st segment. In the process of doing this, care must be taken to avoid look-ahead bias. In particular, if SSA is used to transform all of the data at once, it will take into account all datapoints, including future ones, thus introducing significant look-ahead bias. Therefore, for each test point, we fit SSA to all previous datapoints (including the datapoint in question but none after the datapoint in question) and then make the prediction. Note that this significantly slows down the prediction process but at least it ensures that there is no look-ahead bias. 

We tested both the SVM model and the NB model against each other and against a baseline model which makes predictions entirely based off of trends. We found no statistically significant difference between the baseline and the Naive Bayes model. However, we did find that the SVM model significantly outperformed both the Naive Bayes model and the baseline. As an example, for the facebook data, the SVM model's mean accuracy had a 95% confidence interval of 59 +/- 4% and the baseline's mean accuracy had a 95% confidence interval of 54 +/- 4%, so that the observed mean accuracy of the SVM was above the 95% confidence interval of the baseline's mean accuracy.

## Files in this Repository
This repository contains a Jupyter Notebook called Stock_Direction_Prediction.ipynb which contains the code for feature engineering, model 
training, model assessment and a function called predict_direction which allows you to predict the direction of a stock's movement given the historical data. 

This repository also contains ten year data for various companies. The companies are all major tech companies (Apple, Google, Facebook, Blackberry, Microsoft, IBM and Samsung). Data was collected from Yahoo Finance.

## Future Work
Even though the model clearly outperforms the baseline on the data it was tested on, the model was tested on few stocks and there is no guarantee of its accuracy generalizing to the future. It would be beneficial, therefore, to test it on more data. In the future, it would also be interesting to attempt to incorporate this model into an algorithmic trading model either directly or, perhaps more likely, by combining it with other models in quantitative finance.
