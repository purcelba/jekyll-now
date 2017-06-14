---
layout: post
title: Kaggle challenge | Airbnb New User Bookings
---

Airbnb is an online marketplace that enables people to lease or rent short-term lodging.  The goal of this challenge was to predict which country a new user's first booking destination will be based on a set of demographics and web session records.

# Data overview

The training data set consists of user information collected from 6/28/2010 - 6/30/2014 with the booking destination (target variable) provided.  The test data set consists of the same user information collected from 7/1/2014 - 9/30/2014 with the booking destination to be predicted.  

![Figure 1]({{ site.baseurl }}/images/airbnb_user_data.png "Example user data.")

An additional data set provides web sessions records for a subset of the training and test data and can be linked to the user information via a user id.  

![Figure 2]({{ site.baseurl }}/images/airbnb_sessions.png "Example sessions data.")


Preliminary exploration revealed several interesting and informative aspects of the data.

First, the target variable consisted of 12 possible country destinations making this a multiclass classification problem.  Moreover, the classes are highly unbalanced.  Non-bookings (NDF) and United States bookings (US) vastly outnumber all other classes (58% and 29% of the data set, respectively).  


![Figure 3]({{ site.baseurl }}/images/country_destination_hist.png "Country destination histogram.")


****Insert country_destination histogram*****


Many demographic features are also heavily unbalanced and contain missing values or other unusal observations that must be appropriately handled.

The web sessions data set provides ample opportunities for feature engineering, but are sparse and must be appropriately processes to be useful.  

We can generate a few initial hypotheses about which variables may be most predictive of the users destination.  For example, the age of the user may correlate with income, allowing for more expensive destinatios for older users.  We may also expect the date account created and timestamp first active to be predictive of warmer or colder destinations.  Moreover, if conditions are changing over time

# Feature engineering

