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
##    2    0.9937    0.9920
##   27    0.9921    0.9900
##   52    0.9892    0.9863
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 2.
```







