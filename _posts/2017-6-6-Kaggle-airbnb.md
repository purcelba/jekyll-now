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


# Predicting whether or not a new user will book..

I first trained a model to predict whether or not users placed any booking regardless of the country destination (a binary classification problem).  From a modeling perspective, this simplifies the interpretation of model fits and provide some initial intuition about which features are most important.   From a business perspective, understanding what factors predict whether a user will book can provide valuable insight for targeting potential users even in the absence of destination information.

To validate the models, I held out the last 10% of the training data set collected chronologically (21,345 users).  This followed the test data format which was collected immediately after the training set and ensured that predction accuracy was not inflated by using future data to predict past obsrevations.  The remaining 90% of the data was used for training.  Because the data are unbalanced, I used area under the ROC (auROC) curve for the holdout set to as an error metric to compare models.

For the binary classification, I used L1-regularized logistic regression because it can be trained reasonably fast, provides  interpretable coefficients about feature importance, and establishes a baseline linear model against which more complex algorithms can be compared.  The L1-regularization term was selected using 5-fold cross validation over a range of values.

The logistic regression model accurately predicted whether or not a user would book a destination for 71.4% of the holdout dataset, which exceeds chance (62% in the holdout set).  Accordingly, the auROC curve was 0.75.  In the interest of time, I did not attempt to bootstrap this for a p-value, but the strong performance with a reasonably large holdout set gives some confidence that the model is useful for prediction.  This was later verified by submitted results to the Kaggle website.

The resulting coefficients provide insight about which users will book a destination (Figure 4). For example, users who reach Airbnb via a particular web page (actual URL concealed) or use a Mac Desktop are more likely to book a destination.  Conversely, disengaged users that failed to provide demographic information like gender and age were less likely to book.  These results could inform decisions about where to target advertising for new users or be used to evaluate the success of an ongoing campagin.  

![Figure 4]({{ site.baseurl }}/images/beta_coefficient_histogram.png "Coefficient histogram.")


# Predicting new booking destinations: 

I next moved to the multiclass classification problem of predicting the actual country destination of that the users booked.  I used three algorithms with varying complexity and computational demands: one-versus-rest L1-regularized logistic regression, a boosted trees algorithm implented with XGBoost, and a multi-layer perceptron (MLP) artifical neural network implemented in with Keras. For model comparison, I used macro-averaged auROC which combines auROC for each class with equal weight regardless of the number of observations.  This ensured that my statistic was not dominated by the non-booking class.  I used confusion matrices to determine where the models were succeeding and failing. In addition, I monitored the normalized discounted cumulative gain (NDCG) score, the ranking quality metric used by Kaggle to evaluate submissions.

## L1-regularized logistic regression
I started with a one-versus-next L1-regularized logistic regression approach implemented in scikit-learn.  The model trains a family of logistic regression models to distinguish one class from all others, then chooses the class with the strongers probability.  Model performance is above chance as indicated by a macro auROC of X.  The confusion matrix provides some insight into where the model fails and suceeds.  The model is heavily biased to the NDF and US categories, so there is likely room for improvement.  

## XGBoost
XGBoost is a tree ensembling method that learns a set of decision trees by adding new tree structures that improve a regularized objective function. Unlike logistic regression, XGBoost learns nonlinear decision bounds and is less sensitive to collinearity among features, missing values, or outliers.  The tradeoff is that the model is more complex and the complexity is reflected in an increased number of hyperparameters (e.g., number of trees, learning rate, etc).  I avoided time consuming hand tuning of hyperparameters by using randomized search with 5-fold cross validation.  

XGBoost produced significant improvements in auROC for the holdout dataset, suggesting that it learned generalizable nonlinear relationships beween the features and classes.  We can see in the confusion matrices that the improvement stems primarily from better distinguishing non-bookings and US bookings, but the model still poorly discriminates among non-US bookings.

![Figure 5]({{ site.baseurl }}/images/airbnb_conf_mat.png "Confusion matrices.")

XGBoost also provides an index of feature importance, computed as the reduction in impurity at each node averaged over all trees for each feature.  The results are relatively similar to the logistic regression model.  Unlike the linear model, however, age is by far the most informative feature in the dataset, suggesting that the XGBoost learned a nonlinear relationship between age and booking destination.  Indeed, the data suggest that bookings increase until ages 30-40 and then generally decline thereafter.

![Figure 6]({{ site.baseurl }}/images/auROC.png "auROC histogram.")

![Figure 7]({{ site.baseurl }}/images/pBook_age_bins.png "Age bins.")


## Neural networks: Multi-layer perceptron
To try and improve the overall predictive accuracy, I next trained a multi-layer perceptron (MLP), a class of feedforward neural network models that learns non-linear combinations of features to transform the data into a space where they become lineary separable.  These models are very powerful, but are highly parameterized and require careful training for good results.  I wanted to test whether it could be used to outperform the boosted trees approach by learning novel combinations of features.

I trained the model using mini-batch stochastic gradient descent implemented in Keras with a TensorFlow backend.  To efficiently search the hyperparameter space, I implemented a randomized search with 5-fold cross validation in parallel on a high-performance computing cluster.  This allowed me to test large swaths of hyperparameter space in relatively little time (<5 hours for up to 300 samples simultaneously).  The hyperparameters included the learning rate, neurons per layer, number of hidden layers, activation functions (tanh, sigmoid, linear), dropout rate, momentum, batch size, and number of epochs.  I started by sampling a very broad range of hyperparameters, examined combinations of parameters that produced high cross-validated performance, and then gradually narrowed the search to refine the model fits.  Standard deviation over folds was used to monitor stability.

The neural network model slightly outperformed XGBoost, suggesting that it was able to learn some complex combinations of features without overfitting.  The confusion matrix again suggests slight improvements in discriminating non-bookings and US bookings, but still struggles distinguishing non-US bookings.  This algorithm would be a great choice for situations in which high accuracy is paramount, the combinations of features needed to improve performance is unclear, and long training times are not an issue.  

# Final conclusions

I used a variety of machine-learning approaches to predict where users will book their next destination on Airbnb and learn the features that are most predictive.  The final model was able to provided ranked predictions about where a new user would book that could be used to prioritize listings presented to users.  However, even the best multiclass model struggled to distingiush among non-US bookings and it may be necessary to collect additional user browsing information to boost the signal.  In addition to predictive power, feature selection tools revealed that age is a critical factor in whether a user would book as are the types of websites from which users reach Airbnb.  I also found that inclusion of web sessions data produced modest but reliable improvements in prediction accuracy, which would justify collecting this data to regularly updating models.

For this data set, the nonlinear decision rules learned by the XGB and MLP algorithms improved performance over the linear regularized logistic regression model.  The XGB model was particularly impressive since the time necessary for fitting was quite fast even taking into accoutn the optimization of hyperparameters.  The MLP model ultimately provided the best performance, but took considerably longer to train.  The long training times were greatly reduced by parallelization on a computing cluster, making this an attractive option if a cluster is available.

Using the MLP model, my final NDSG score on the Kaggle private leaderboard was 0.88209 - not too far from the winning score of 0.88697 (range = 0 to 1).  As noted on the Kaggle blog (LINK), the top models in this competition stacked and ensembled up to 15 models.  I explored one simple stacking model using predictions of the binary linear classifier as a feature for multiclass classification.  I observed a slight improvement in fitting, suggesting that additional ensembling and stacking may be a fruitful avenue to pursue in future projects.  Complex stacking of many models is often impractical in many business applications, but it may be useful for cases in which small gains in accuracy translate to substantial real-world gains.






http://blog.kaggle.com/2016/03/17/airbnb-new-user-bookings-winners-interview-2nd-place-keiichi-kuroyanagi-keiku/)






