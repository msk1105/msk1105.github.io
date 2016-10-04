---
layout: post
title: "Dental Risk Predictor"
---

<left> <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTL2JxHKdQjNLC5WFsu-MVkCbybdOOi2fsD4gQnS44x65m72sUO" alt="alt text" width="350px"> </left>

In this post, I would like to share results of my machine learning project, **"Dental Risk Predictor"**, at [Insight Data Science](http://insightdatascience.com/), as well as discussing their usefulness in dental practice.

I am a research scientist to study [neurodevelpmental disorders](https://en.wikipedia.org/wiki/Neurodevelopmental_disorder), and the idea of my project came from my longstanding interest in translating research findings into clinical practice.  


### A Problem Inherent in Dental Practice

Dental patients often cancell or do not show up for their appointments, costing dental practices thousands of dollars. One of the reasons for cancellation or no show-up is that patients have **NO PERCEIVED NEED**. 


### A Potential Solution
To correct this problem, dentists can proactively educate patients about **potential risks for having dental problems**, which will help to fill a gap between their perceived need and the actual need. To provide dentists with a tool for estimaging patients' dental risks, I built models to predict **the probability of present/lost tooth** in each location. Note that most adults have 32 teeth.


### Building Predictive Models 

A question here is “What features can predict whether or not a patient will lose a permanent tooth in each location?". See the figure below, illustrating the human teeth anatomy. 

<center> <img src=
"http://i1376.photobucket.com/albums/ah16/makisophiakoyama/teeth_picture_zpswkoujdtm.png" alt="alt text" height="300px"> </center>.

To solve this problem, I leverage the publicly available dataset, [National Health and Nutrition Examination Survey (NHANES)2007- 2008](http://www.icpsr.umich.edu/icpsrweb/DSDR/studies/25505). 

**The Outcome** is a binary variable with two classes, permanent tooth "present" or "absent", in each location ("0" for tooth absent and "1" for tooth present). Severn **Features** are included:  1) age, 2) gender, 3) insurance, 4) income, 5) smoking now, 6) smoked previously, 7) alchol drinking now, 8) alcohol drinking previously, 9) avoided some food due to teeth problems in the last 12 months.

Because the outcome variable is binary, we use logistic regression as a classifier to model the probability. That is, we are estimating the probablity that the model can accurately classify the outcome, where 1 = "tooth present" and 0 = "tooth absent", given certain values of each feature.

The following is the logistic formula to calculate the probability (p) that Y=1 for a given value of X. The probability that Y=0 is 1-p. 

<center> <img src=
"http://taf-website-backup.s3.amazonaws.com/logit.png" alt="alt text" height="100px"> </center>
                
For this analysis, I used 
[scikit-learn](learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html), a machine learning library for Python. 


### Results and Cross Validation

I short, one model including 4 features (income, smoking now, smoking previously, avoided food) yeilded a high classificaiton probability for each of the 32 tooth locations, ranging between. When interpreting the coefficients at face value, **"higher income"** is associated with a higher probability of maintaining a permanent tooth in each location, while "current smoking", "previous smoking", and "avoided food" are associated with a lower probablity of maintaining it. 

Now, I test how well my model ("learner") can make a new prediction for data it has not already seen. For this, I use [cross validation](http://scikit-learn.org/stable/modules/cross_validation.html). As I want to use all of the data, I split the data into 70% for training and 30% for testing, and then perform 10-fold cross validation (i.e., training the model 10 times on 70% of the data and testing on 30%). 

To visualize how well a model can distinguish between two classes(tooth present/absent), I plot a relative operating characteristic curve (ROC), illustrating the cross validation result for the tooth #2. In general, AUC values of 0.70 and higher would be considered strong effects. 

<center> <img src="{{ site.baseurl }}/images/ROC_AUC.png" alt="alt text" width="600px"> </center>

Interestingly, the AUC value is relatively high for each **wisdome tooth** (0.72 ~ 0.75), but the direction of the effect is opposite to other teeth: **"higher income"** and **"having dental insurance"** are associated with a lower probability of maintaining each widsom tooth. This is not surprising considering that those with dental insurance are more likely to go to the dentist and comply with the recommendations, including wisdom teeth extraction.  

<center> <img src="{{ site.baseurl }}/images/roc.png" alt="alt text" width="600px"> </center>

Finally, I am now developing an app that allows dentists to collect patients' information to predict potential dental risks. The collected information can be used to constantly update and improve the model(s) through ongoing modifications. This app can gives dentists the ability to educate and motivate patients to maintain dental visits and comply with dental treatment or prevention recommendations. 


