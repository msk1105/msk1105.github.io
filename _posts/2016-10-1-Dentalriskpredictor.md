---
layout: post
title: "Dental Risk Predictor"
"Keeping Appointments, Keeping Healthy Smiles"
---

<left> <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTL2JxHKdQjNLC5WFsu-MVkCbybdOOi2fsD4gQnS44x65m72sUO" alt="alt text" width="350px"> </left>

In this post, I would like to share results of my project using machine learning at [Insight Data Science](http://insightdatascience.com/), as well as discussing their usefulness in dental practice.

I am a research scientist to study [neurodevelopmental disorders](https://en.wikipedia.org/wiki/Neurodevelopmental_disorder), and the idea of my project came from my longstanding interest in translating research findings into clinical practice.  



## A Problem Inherent in Dental Practice
Dental patients often cancell or do not show up for their appointments, costing dental practices thousands of dollars. One of the reasons for cancellation or no show-up is that patients have **no perceived need**. 



## A Potential Solution
To correct this problem, dentists can proactively educate patients about **potential risks for having dental problems**, which will help to fill a gap between their perceived need and the actual need. To provide dentists with a tool for estimaging patients' dental risks, I built models to predict **the probability of a tooth being  present or absent** in each tooth location, based on patients' demographic and lifesylte information. 


## Building Predictive Models 
To solve this problem, I leverage the publicly available dataset, [National Health and Nutrition Examination Survey (NHANES)2007- 2008](http://www.icpsr.umich.edu/icpsrweb/DSDR/studies/25505). 

The left figure below illustrates the dental anatomy, and most adults have 32 teeth, including four wisdom teeth. The right figure below summarizes the percentage (%) of adults having a permanent tooth in each location (based on the NHANES data). Consistent with 

<center> <img src="{{ site.baseurl }}/images/dental_distribution.png" alt="alt text" width="600px"> </center>


**The Outcome** is a binary variable with two classes, permanent tooth "present" or "absent", in each location. 

**Nine Features** include:  1) age, 2) gender, 3) insurance, 4) income, 5) smoking now, 6) smoked previously, 7) alchol drinking now, 8) alcohol drinking previously, and 9) avoided some food due to teeth problems in the last 12 months.

Because the outcome variable is binary, we use logistic regression as a [discriminative classifier](https://www.cs.cmu.edu/~tom/mlbook/NBayesLogReg.pdf) to model the probability. That is, we are estimating the probablity that the model can accurately classify the outcome, where 1 = "tooth present" and 0 = "tooth absent", given certain values of each feature.

The following is the logistic formula to calculate the probability (p) that Y=1 for a given value of X. The probability that Y=0 is 1-p. 

<center> <img src=
"http://taf-website-backup.s3.amazonaws.com/logit.png" alt="alt text" height="75px"> </center>
                
For this analysis, I mainly use
[scikit-learn](learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html), a machine learning library for Python. 



## Results and Cross Validation
In short, one model including 4 features (income, smoking now, smoking previously, avoided food) yeilds a high classificaiton probability for each of the 32 tooth locations, in the range between 0.72 and 0.84. When interpreting the coefficients at face value, **"higher income"** is associated with a higher probability of a tooth being present in each location, while **"current smoking", "previous smoking", "avoided food", and "lower income"** are associated with a lower probablity of a tooth being present, that is, **higher risk of losing a tooth**.  

Now, I test how well my model ("learner") can make a new prediction for data it has not already seen. For this, I use [cross validation](http://scikit-learn.org/stable/modules/cross_validation.html). As I want to use all of the data, I split the data into 70% for training and 30% for testing, and then perform 10-fold cross validation (i.e., training the model 10 times on 70% of the data and testing on 30%). 

To visualize how well the model can distinguish between two classes (tooth present or absent), I plot a relative operating characteristic curve (ROC curve) from the cross-validation results of the model. The Area Under the Curve (AUC), a quantitative measure of the discriminative performance, is high for each tooth, in the range of between 0.71 and 0.84. For example, the ROC below shows the result of the tooth #2 (AUC=0.82), suggesting that the model is able to accurately clasify the outcome in this tooth location (AUC for a perfect classifier is 1).

<center> <img src="{{ site.baseurl }}/images/ROC_AUC.png" alt="alt text" width="400px"> </center>

It is worht noting here that the AUC value is relatively high for each **wisdom tooth** (0.72 ~ 0.75), but the direction of the effect is opposite to other teeth: **"higher income"** and **"having dental insurance"** are associated with a lower probability of a wisdom tooth being present. This is not surprising considering that patients with dental insurance are more likely to go to the dentist and comply with the recommendations, including wisdom teeth extraction.  



## The Upcoming App

I am now developing **an app** that allows dentists to collect patients' information to predict potential dental risks. 

<center> <img src="{{ site.baseurl }}/images/dental_app.png" alt="alt text" width="800px"> </center>

This app can gives dentists the ability to educate and motivate patients to maintain dental visits and comply with dental treatment or prevention recommendations. Finally, **the main take-away message** in this project is that dentists should collect the information about patients' oral hygienic habits (not included in the data used in this project) to improve predictive model(s) through ongoing modifications. 


## My Presentation 

<center> <iframe src="//www.slideshare.net/slideshow/embed_code/key/2gJ94ezFc59xfn" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/AlinePichon/aline-pichon-demonoaddslides" title="Aline pichon demo_no_addslides" target="_blank">Aline pichon demo_no_addslides</a> </strong> from <strong><a href="//www.slideshare.net/AlinePichon" target="_blank">Aline Pichon</a></strong> </div> </center>


