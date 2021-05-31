# JOBATHON 2 CREDIT CARD LEAD PREDICTION

**Approach to the problem**

**1.Business Problem:**

As part of the competetion we are given a set of features about an user we have to predict whether that user is a potential lead for credit card or not.

**2.Performance Metric:**

The metric for evaluating performance in competetion would AUC.’

**3.Data:**

The data was given as part of the competetion. Looking at the data we have 10 features detailing the banking details of an user. The class labels are imbalanced with ratio of almost 3:1. 


**4.Exploratory Data Analysis:**

We took each feature one by one and verified it in comparison to the target label. Some key  information derived include:

- For Females credit card lead count is slightly less than in comparison to males

- Almost all entrepreneurs have credit cards and hence can be consider as potential lead

-Distribution of Vintage for positive leads to credit card has its median much higher than that of non leads.

**5.Preprocessing Data:**

I have done following encodings to the data as part of my preprocessing.

Label Encoded: Gender,Credit_Product,Is_Active

Reason: Categories in features have boolean relationship i.e. 0 or 1. So i have enccoded them as such.

Onehot_Coded: Occupation,Channel_Code

Reason: Here the number of categories increased so i represented each category in one vs all manner.

Response_Coded: Region_Code

Reason: I have used P(Is_Lead=1|Category in Region code) and P(Is_Lead=0|Category in Region code). The reason is the number of unique caegories are slightly higher and it is better to have less dimensional features for tree based models.



**Missing Value Imputation:**

Only the Credit_Product feature contained missing values so i decided to do a model based imputation to the feature.I labelled the missing values as the test set. I trained a simple Xgboost model on train set i.e. the datapoints with Credit_Product values available. The model gave an AUC of 0.71. Then i imputed the missing values by predicting on the test set.

**6.Feature Engineering**

I was able to come up with only one good engineered feature, i.e. Credit_NaN. It is a boolean feature that represents whether the Credit_Product value is present or is it missing.

**7. Modelling**

For modelling i did a train test split and started with the Random Forest model. The model performed well with an AUC of 0.86. From confusion matrix we saw that model is having a lower recall on positive class labels. This may happen due to class imbalance. To address the imbalance i also tried class balancing techniques like oversampling and undersampling but they didn’t seemed to be much benefecial so i dropped those ideas.

Here is a plot of feature importances..

![image](https://user-images.githubusercontent.com/77936888/120152468-266c6980-c20b-11eb-83b8-b23e9ca07d7d.png)

The engineered feature seems to be the most important feature in our prediction..

Then I built a XGBoost model and the performance improved slightly. So i decided to hyperparameter tune this Xgboost model. For this i have used a Random Search CV model and found the best hyperparams. Using the best hyperparams i trained the model and got the final AUC score of 0.87476. 

Using this model i submitted the predictions in the site and it gave an AUC of 0.8730 and i got a rank of 191th in private leaderboard which lies in top 7%.

**What further can be done? **

- In depth Error analysis
Resampled all the data points where the model failed to give correct prediction. Add them twice to the dataset to increase their weights.

- Literature survey
We can do a thorough literature survey to find what other interesting features we can engineer as we can see the model is clearly lacking engineered features.


Thank You
