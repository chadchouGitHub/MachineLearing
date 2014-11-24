# Data Scienc
Chia-Ching Chou  
November 23, 2014  
Prediction Assignment Writeup

#Instroduction:

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their
behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways


#Data
The data for this project come from this source: http://groupware.les.inf.puc-rio.br/har (http://groupware.les.inf.pucrio.br/har).

#Process Data
Read trainning data and clean missing data.

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
```

Data splitting with for traning data and cross validation (7:3).

```r
inTrain<-createDataPartition(y=trainDataFull$classe,p=0.7,list=FALSE)
trainData<-trainDataFull[inTrain,]
cvData<-trainDataFull[-inTrain,]
```

#Model
Use "rf" model


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
##    2    0.9925    0.9905
##   27    0.9924    0.9903
##   52    0.9870    0.9836
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 2.
```

#Validation


```r
cv_predictions<-predict(modelFit,cvData[,-53])
confusionMatrix(cv_predictions,cvData$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1668    9    0    0    0
##          B    3 1127   10    0    0
##          C    0    3 1015   16    2
##          D    0    0    1  946    2
##          E    3    0    0    2 1078
## 
## Overall Statistics
##                                         
##                Accuracy : 0.991         
##                  95% CI : (0.989, 0.994)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.989         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.996    0.989    0.989    0.981    0.996
## Specificity             0.998    0.997    0.996    0.999    0.999
## Pos Pred Value          0.995    0.989    0.980    0.997    0.995
## Neg Pred Value          0.999    0.997    0.998    0.996    0.999
## Prevalence              0.284    0.194    0.174    0.164    0.184
## Detection Rate          0.283    0.192    0.172    0.161    0.183
## Detection Prevalence    0.285    0.194    0.176    0.161    0.184
## Balanced Accuracy       0.997    0.993    0.992    0.990    0.998
```

#Test


```r
pmlTest <- read.csv("pml-testing.csv", na.strings=c("NA",""))
testData<-pmlTest[,names(pmlTest)%in%names(trainDataFull)]
predictions<-predict(modelFit,testData)
predictions
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```



