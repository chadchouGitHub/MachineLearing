# Data Scienc
Chia-Ching Chou  
November 23, 2014  
Prediction Assignment Writeup

#





```r
library(caret)
```

```
## Warning: package 'caret' was built under R version 3.1.2
```

```
## Loading required package: lattice
## Loading required package: ggplot2
```

```r
#read trainning data and remove missing values
inTrainData <- read.csv("pml-training.csv",na.strings=c("NA",""))
missingVals<-apply(inTrainData,2, function (x) {sum (is.na(x))})
tr_temp<-inTrainData[,missingVals==0]
trainDataFull<-tr_temp[,-(1:7)]
#data splitting with 70% for traning data and 30% for cross validation.
inTrain<-createDataPartition(y=trainDataFull$classe,p=0.7,list=FALSE)
trainData<-trainDataFull[inTrain,]
cvData<-trainDataFull[-inTrain,]
```




```r
modelFit<-train(trainData[,-53],trainData$classe,method="rf",trControl=trainControl(method="oob",number=4,allowParallel=TRUE))
```

```
## Loading required package: randomForest
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```

```r
modelFit
```

```
## Random Forest 
## 
## 13737 samples
##    52 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy  Kappa 
##    2    0.9934    0.9917
##   27    0.9924    0.9904
##   52    0.9882    0.9851
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 2.
```


```r
cv_predictions<-predict(modelFit,cvData[,-53])
confusionMatrix(cv_predictions,cvData$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1673    3    0    0    0
##          B    0 1135    3    0    0
##          C    0    1 1023   21    0
##          D    0    0    0  943    3
##          E    1    0    0    0 1079
## 
## Overall Statistics
##                                         
##                Accuracy : 0.995         
##                  95% CI : (0.992, 0.996)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.993         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.999    0.996    0.997    0.978    0.997
## Specificity             0.999    0.999    0.995    0.999    1.000
## Pos Pred Value          0.998    0.997    0.979    0.997    0.999
## Neg Pred Value          1.000    0.999    0.999    0.996    0.999
## Prevalence              0.284    0.194    0.174    0.164    0.184
## Detection Rate          0.284    0.193    0.174    0.160    0.183
## Detection Prevalence    0.285    0.193    0.178    0.161    0.184
## Balanced Accuracy       0.999    0.998    0.996    0.989    0.999
```





