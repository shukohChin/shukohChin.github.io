---
layout: post
title: Logistic Regression
category: DataScience
tags: [R, The_Analytics_Edge, edx]
---

<!-- more -->

### 1. basic
  - binary variable: 非此即彼的变量，包含在categorical variable里  
  - 公式: $$P(y = 1) = \frac{1}{1+e^{-(\beta\_0+\beta\_1x\_1+\beta\_2x\_2+...+\beta\_kx\_k)}}$$
  
![スクリーンショット 2015-06-24 0.04.21.png](https://qiita-image-store.s3.amazonaws.com/0/44948/794e3382-bd48-bcd7-35ea-4688f2fbcc37.png "スクリーンショット 2015-06-24 0.04.21.png")
  
  - Odds
  $$Odds = \frac{P(y = 1)}{P(y = 0)}$$  
  Odds > 1 if y = 1 is more likely  
  Odds < 1 if y = 0 is more likely  
  - Logit $log(Odds)$  
  $$Odds = e^{\beta\_0 + \beta\_1x\_1 + \beta\_2x\_2 + ... + \beta\_kx\_k}$$  
  $$log(Odds)=\beta\_0 + \beta\_1x\_1 + \beta\_2x\_2 + ... + \beta\_kx\_k$$
  
### 2. create logistic model
- spit train&test set  

```r
install.packages("caTools")
set.seed(88)
split = sample.spit(data$column, SplitRatio = 0.75)  
#75%<-training set, 25%<-testing set
dataTrain = subset(data, split == TRUE)  
dataTest = subset(data, split == FALSE)  
```

- create logistic model  

```r
logisModel = glm(column ~ independent variables, data = dataTrain, family=binomial)
```

- predict

```r
predictTrain = predict(logisModel, newdata=dataTest, type="response")
tapply(predictTrain, dataTrain$column, mean)
```

- select a Threshold value
    + sensitivity  $\frac{TP}{TP+FN}$  
    + specificity  $\frac{TN}{TN+FP}$  
        threshold higher -> sensitivity lower  
        threshold lower -> sensitivity higher

|          |   Predicted=0        |     Predicted=1      |
|:---------|:--------------------:|:--------------------:|
| Actual=0 |  True Negatives(TN)  |  False Positives(FP) |
| Actual=1 |  False Negatives(FN) |  True Positives(TP)  |
  
### 3. ROC Curve
![kobito.1435571623.819722.png](https://qiita-image-store.s3.amazonaws.com/0/44948/372454ed-cc36-d7bc-2c73-c0f04d064d51.png "kobito.1435571623.819722.png")

- parameter  
    + True positive rate -> **sensitivity**  
        proportion of poor care caught
    + False positive rate -> **1-specificity**  
        proportion of good care labeled as poor care

- create ROC Curve

```r
install.packages("ROCR")
library(ROCR)
ROCRpred = prediction(predictTrain, dataTrain$column)
#first arg: predictions we made using model
#second arg: true outcomes of data
ROCRperf = performance(ROCRpred, "tpr", "fpr")
plot(ROCRperf)
plot(ROCRperf, colorize=TRUE, print.cutoffs.at=seq(0,1,0.1), text.adj=c(-0.2, 1.7))
```

- **AUC(Area under the ROC Curve)**

```r
predictTest=predict(logisModel, type="response",newdata=dataTest)
ROCRpredTest=prediction(predictTest, dataTest$column)
auc=as.numeric(performance(ROCRpredTest,"auc")@y.values)
```


