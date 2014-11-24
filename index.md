---
title       : Birthweight Prediction
subtitle    : Birthweight prediction based on the condition of the mother.
author      : Ruben-S
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Overview of the Birthweight Prediction App

This slide-deck gives the background on a Shiny application created to predict the birthweight of a baby, based on the condition of the mother.

#### Note1:
This app was made as an assignment for the 'Data Science Course' run by 'John Hopkins Bloomberg School of Public Health' on Coursera.

#### Note2:
- The data used for creating the predictionmodel was sourced from:  
  Hosmer, D.W., Lemeshow, S. and Sturdivant, R.X. (2013);  
  Applied Logistic Regression: Third Edition.  
  These data are copyrighted by John Wiley & Sons Inc. and must be acknowledged and used accordingly.  
  Data were collected at Baystate Medical Center, Springfield, Massachusetts during 1986.
- The data was retrieved from http://www.umass.edu/statdata/statdata/data/lowbwt.txt

--- .class #id 

## Set-Up & Model

- The dataset was quite small: 189 observations. As the dataset is small,
the data was included in the server.R file of the Shiny App.
- The data set contained 8 variables & 2 outcomes:
  * 1 outcome is the classifiction if the observation was a low birthweight (< 2500 grams)
  * 1 outcome gave the actual recorded birhtweight
  * 8 variables on the condition of the mother (see following slide)
- The data set was split in a training set (3/4) & test set (1/4).  
    
    ```r
    inTrain = createDataPartition(data$birthweight, p = 3/4)[[1]]
    ```
- A randomforrest model was built on the training.
    
    ```r
    modfit_rf <- train(birthweight ~ ., method  = 'rf', data = trainData)
    ```
- A standard deviation was calculated based on the prediction with the test set.

--- .class #id 

## Input

* The following inputs are provided in the Shiny app to indicate the condition of the mother:
  * Age
  * Ethnic origin
  * Number of Physician Visits During the First Trimester
  * Weight in Pounds at the Last Menstrual Period
  * Smoking Status During Pregnancy
  * History of Premature Labor 
  * History of Hypertension
  * Presence of Uterine Irritability 
* The inputs were captured with checkboxInputs, SliderInput and RadioButtons

--- .class #id 

## Prediction

* Based on the input the Shiny app uses the caret library function predict to give an indication of the birthweight.
    
    ```r
    predict(modfit_rf, newdata = participant1)
    ```
* Due to the low number of observations the model exhibits quite a large standard deviation. The standard deviation calculated on the test set was around 700 grams.
* More observations will be needed to increase the accuracy of the prediction.



