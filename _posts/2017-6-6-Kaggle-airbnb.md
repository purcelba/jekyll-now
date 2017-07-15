---
layout: post
title: Kaggle challenge | Airbnb New User Bookings
---

Airbnb is an online marketplace that enables people to lease or rent short-term lodging.  The goal of this challenge was to predict which country a new Airbnb user's first booking destination will be based on a set of demographics and web session records. Additionally, I used model-based appraoches to explore and understand which user factors were most predictive about whether and where a user would book.

# Data overview

The training data set consists of user information collected from 6/28/2010 - 6/30/2014 with the booking destination (target variable) provided (213,451 users).  The goal of the channelge is to preict the booking destination for a test data set collected from 7/1/2014 - 9/30/2014 (62,096 users).  

![Figure 1]({{ site.baseurl }}/images/airbnb_user_data.png "Example user data.")

An additional data set provides web sessions records that can be linked to the user information tables via a user id.  Each record includes an action that was taken defined by the action name, type, and detail.  The number of seconds spent on the action are also provided.  

![Figure 2]({{ site.baseurl }}/images/airbnb_sessions.png "Example sessions data.")

The target variable consisted of 12 possible country destinations making this a multiclass classification problem.  The classes are highly unbalanced.  Non-bookings (NDF) and United States bookings (US) vastly outnumber all other classes (58% and 29% of the data set, respectively).  Many demographic features are also heavily unbalanced and contain missing values or other unusal observations that must be appropriately handled.

![Figure 3]({{ site.baseurl }}/images/country_destination_hist.png "Country destination histogram.")

# Data preparation

I formatted and cleaned the user information to be suitable for modeling.  I checked the distributions of all user features.  Dates were formatted into years, months, days, and weeks of the year.  Ages that were likely to be errors (<14 or >100) were recoded into missing values.  Missing values were not imputed, but were coded as zero and a new feature was added to indicate missing values.  One-hot-feature encoding was used to generate features for all categorical variables.  All features were rescaled between zero and one so that features with higher magnitudes did not dominate the objective functions for certain algorithms.

For the web sessions data, I encoded each action name, type, and detail as a separate feature.  These were binary variables to indicate whether or not the user took a particular action at any time.  I also explored versions that considered the number of times a user took each action and the total time spent on each action, but found no benefit of the added information.


# Predicting whether a new user will book.

I first trained a model to predict whether or not users placed any booking regardless of the country destination (a binary classification problem).  To validate the models, I held out the last 10% of the training data set collected chronologically (21,345 users).  This followed the test data format which was collected immediately after the training set and ensured that predction accuracy was not inflated by using future data to predict past obsrevations.  The remaining 90% of the data was used for training.  Because the data are unbalanced, I used area under the ROC (auROC) curve for the holdout set to as an error metric to compare models.

For the binary classification, I used L1-regularized logistic regression because it can be trained reasonably fast, provides  interpretable coefficients about feature importance, and establishes a baseline linear model against which more complex algorithms can be compared.  The regularization term penalizes the model for higher coefficient values (in particular non-zeros) to prevent overfitting.  The regularization term was selected using 5-fold cross validation over a range of values.

The logistic regression model accurately predicted whether or not a user would book a destination for 71.4% of the holdout dataset, which exceeds chance (62% in the holdout set).  Accordingly, the auROC curve was 0.75.  The resulting coefficients provide insight about which users will book a destination (Figure 4). For example, users who reach Airbnb via a particular web page (actual URL concealed) or use a Mac Desktop are more likely to book a destination.  Conversely, disengaged users that failed to provide demographic information like gender and age were less likely to book.

![Figure 4]({{ site.baseurl }}/images/beta_coefficient_histogram.png "Coefficient histogram.")


# Predicting booking destinations: 

I next moved to the multiclass classification problem of predicting the actual country destination of that the users booked.  I used three algorithms with varying complexity and computational demands. For model comparison, I used macro-averaged auROC which combines auROC for each class with equal weight regardless of the number of observations.  I used confusion matrices to determine where the models were succeeding and failing. I also monitored the normalized discounted cumulative gain (NDCG) score, the ranking quality metric used by Kaggle to evaluate submissions. The results are summarized at the end of this section.

## L1-regularized logistic regression
I started with a one-versus-next L1-regularized logistic regression approach implemented in scikit-learn.  The model trains a family of linear classifiers to distinguish one class from all others, then chooses the class with the highest probability.  The L1-regularization model performance is above chance as indicated by a macro auROC of X.  The confusion matrix indicates that the model is heavily biased to the NDF and US categories.  

## XGBoost
XGBoost is a tree ensembling method that learns a set of decision trees by asking whether new tree structures will improve a regularized objective function. Unlike logistic regression, XGBoost learns nonlinear decision bounds, but relies on more hyperparameters (e.g., number of trees, learning rate, etc).  I avoided hand tuning of hyperparameters by using randomized search with 5-fold cross validation.  The resulting model  produced a notable improvement in auROC for the holdout dataset. XGBoost also provides an index of feature importance, computed as the reduction in impurity at each node averaged over all trees for each feature.  Unlike the linear model, the XGBoost model learned a nonlinear relationship between age and booking that was evident in the data.

![Figure 5]({{ site.baseurl }}/images/xgb_feature_importance_barh.png "Age bins.")

![Figure 6]({{ site.baseurl }}/images/pBook_age_bins.png "Age bins.")

## Neural networks: Multi-layer perceptron
Multi-layer perceptrons (MLP) are a class of feedforward neural network models that learns combinations of features to transform the data into a space where they become lineary separable.  These models are very powerful, but  highly parameterized and require careful training for good results.  I trained the model using mini-batch stochastic gradient descent implemented in Keras with a TensorFlow backend.  To efficiently select hyperparameters, I implemented a randomized search with 5-fold cross validation in parallel on a high-performance computing cluster.  The trained network outperformed both logistic regression and boosted trees on the holdout set.  This algorithm would be a great choice for situations in which high accuracy is paramount, but long training times are not an issue.  

![Figure 7]({{ site.baseurl }}/images/airbnb_conf_mat.png "Confusion matrices.")

![Figure 8]({{ site.baseurl }}/images/auROC.png) { width: 100px; }


# Conclusions

I used a variety of machine-learning approaches to predict where users will book their next destination on Airbnb and learn the features that are most predictive.  The final model provided ranked predictions about where a new user would book that could be used to prioritize listings shown to users.  Feature selection tools revealed that age and previously visited web sites are critical factors in whether a user will book.  These results could inform decisions about where to advertise for new users or to evaluate ongoing advertising campagins.

For this data set, the XGB and MLP algorithms that used nonlinear decision rules outperformed the linear model.  The XGB model was particularly impressive since the time necessary for fitting was quite fast.  The MLP model provided the best performance, but took considerably longer to train.  The long training times were greatly reduced by parallelization on a computing cluster (<5 hours total, up to 300 samples in parallel), making this an attractive option if a cluster is available.

Using the MLP model, my final NDSG score on the Kaggle private leaderboard was 0.88209 - close to the winning score of 0.88697 (range = 0 to 1).  The top models in this competition stacked and ensembled up to 15 models (see interviews with the [2nd](http://blog.kaggle.com/2016/03/17/airbnb-new-user-bookings-winners-interview-2nd-place-keiichi-kuroyanagi-keiku/) and [3rd](http://blog.kaggle.com/2016/03/07/airbnb-new-user-bookings-winners-interview-3rd-place-sandro-vega-pons/) place finishers).  I explored one simple stacking model using predictions of the binary linear classifier as a feature, which produced slight improvement in prediction accuracy. Complex stacking of many models is often impractical in many business applications, but it may be useful for cases in which small gains in accuracy translate to substantial real-world gains.













