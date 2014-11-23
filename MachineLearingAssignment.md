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





Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
