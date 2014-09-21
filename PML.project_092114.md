
title: Practical Machine Learning Project
=========================================



##Downloading the data  

```r
setwd("C:\\Users\\hang\\Desktop\\Coursera_5Top\\PracticalMachineLearning\\Project\\") 
```

```
## Error: cannot change working directory
```

```r
if(!file.exists("data")){
	dir.create("data")
}
##downloading "training" data  
fileURL<-"http://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv" ## replace "https" to "http" then worked!
download.file(fileURL, destfile="./data/training.csv")
dateDownloaded<-date()
dateDownloaded
```

```
## [1] "Sun Sep 21 08:22:48 2014"
```

```r
##downloading "testing" data  
fileURL<-"http://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv" ## replace "https" to "http" then worked!
download.file(fileURL, destfile="./data/testing.csv")
dateDownloaded<-date()
dateDownloaded
```

```
## [1] "Sun Sep 21 08:22:48 2014"
```

##Loading the "training" and "testing" data.     


```r
# The "training" data which is named as "d".
d<-read.csv("./data/training.csv", header=TRUE)
dim(d)
str(d) # 19622 obs. of 160 vars
#summary(d) # the last var "classe" is outcome to be predicted.

# The "testing" data which is named as "d2".
d2<-read.csv("./data/testing.csv", header=TRUE)
dim(d2)
#str(d2) # 20 obs. of 160 vars
#summary(d2) # the last var "classe" is outcome to be predicted.
```

##Cleaning & Exploring Data  
I noticed that there are many variables that contain a large number of NAs, several variables have value "DIV/0!". To review the data in details, I used Hmisc package and options() function.  

```r
# NA details
library(Hmisc) 
options(na.action="na.keep", na.detail.response=TRUE)
# review the dataframe "d".
a<-describe(d)
a[1:20]
```

d 

 20  Variables      19622  Observations
---------------------------------------------------------------------------
X 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0   19622    9812   982.1  1963.1  4906.2  9811.5 14716.8 
    .90     .95 
17659.9 18641.0 

lowest :     1     2     3     4     5
highest: 19618 19619 19620 19621 19622 
---------------------------------------------------------------------------
user_name 
      n missing  unique 
  19622       0       6 

          adelmo carlitos charles eurico jeremy pedro
Frequency   3892     3112    3536   3070   3402  2610
%             20       16      18     16     17    13
---------------------------------------------------------------------------
raw_timestamp_part_1 
        n   missing    unique      Mean       .05       .10       .25 
    19622         0       837 1.323e+09 1.322e+09 1.322e+09 1.323e+09 
      .50       .75       .90       .95 
1.323e+09 1.323e+09 1.323e+09 1.323e+09 

lowest : 1322489605 1322489606 1322489607 1322489608 1322489609
highest: 1323095077 1323095078 1323095079 1323095080 1323095081 
---------------------------------------------------------------------------
raw_timestamp_part_2 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0   16783  500656   48389  100343  252912  496380  751891 
    .90     .95 
 900367  950649 

lowest :    294    301    307    309    312
highest: 998716 998741 998749 998750 998801 
---------------------------------------------------------------------------
cvtd_timestamp 
      n missing  unique 
  19622       0      20 

lowest : 02/12/2011 13:32 02/12/2011 13:33 02/12/2011 13:34 02/12/2011 13:35 02/12/2011 14:56
highest: 28/11/2011 14:14 28/11/2011 14:15 30/11/2011 17:10 30/11/2011 17:11 30/11/2011 17:12 
---------------------------------------------------------------------------
new_window 
      n missing  unique 
  19622       0       2 

no (19216, 98%), yes (406, 2%) 
---------------------------------------------------------------------------
num_window 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0     858   430.6      44      88     222     424     644 
    .90     .95 
    780     821 

lowest :   1   2   3   4   5, highest: 860 861 862 863 864 
---------------------------------------------------------------------------
roll_belt 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0    1330   64.41   -0.20    0.53    1.10  113.00  123.00 
    .90     .95 
 129.00  139.00 

lowest : -28.9 -28.8 -28.6 -28.4 -28.3
highest: 158.0 159.0 160.0 161.0 162.0 
---------------------------------------------------------------------------
pitch_belt 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0    1840  0.3053  -43.60  -42.10    1.76    5.28   14.90 
    .90     .95 
  25.40   26.20 

lowest : -55.8 -54.9 -54.7 -54.4 -53.9
highest:  59.9  60.0  60.1  60.2  60.3 
---------------------------------------------------------------------------
yaw_belt 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0    1957  -11.21   -93.5   -92.9   -88.3   -13.0    12.9 
    .90     .95 
  165.0   168.0 

lowest : -180 -179 -178 -177 -176, highest:  175  176  177  178  179 
---------------------------------------------------------------------------
total_accel_belt 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
  19622       0      29   11.31       2       3       3      17      18 
    .90     .95 
     20      21 

lowest :  0  1  2  3  4, highest: 25 26 27 28 29 
---------------------------------------------------------------------------
kurtosis_roll_belt 
      n missing  unique 
  19622       0     397 

lowest :           -0.016850 -0.021024 -0.025513 -0.033935
highest: 5.587755  5.681869  6.545935  7.004355  7.515290  
---------------------------------------------------------------------------
kurtosis_picth_belt 
      n missing  unique 
  19622       0     317 

lowest :           -0.021887 -0.060755 -0.099173 -0.108371
highest: 8.953960  9.042959  9.296951  9.804491  9.896970  
---------------------------------------------------------------------------
kurtosis_yaw_belt 
      n missing  unique 
  19622       0       2 

 (19216, 98%), #DIV/0! (406, 2%) 
---------------------------------------------------------------------------
skewness_roll_belt 
      n missing  unique 
  19622       0     395 

lowest :           -0.003095 -0.010002 -0.014020 -0.015465
highest: 2.058296  2.097857  2.674649  2.713152  3.595369  
---------------------------------------------------------------------------
skewness_roll_belt.1 
      n missing  unique 
  19622       0     338 

lowest :           -0.005928 -0.005960 -0.008391 -0.017954
highest: 6.164414  6.708204  6.782330  6.855655  7.348469  
---------------------------------------------------------------------------
skewness_yaw_belt 
      n missing  unique 
  19622       0       2 

 (19216, 98%), #DIV/0! (406, 2%) 
---------------------------------------------------------------------------
max_roll_belt 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
    406   19216     195  -6.667  -93.07  -92.30  -88.00   -5.10   18.50 
    .90     .95 
 165.50  169.00 

lowest : -94.3 -94.1 -93.9 -93.8 -93.6
highest: 172.0 173.0 174.0 177.0 180.0 
---------------------------------------------------------------------------
max_picth_belt 
      n missing  unique    Mean     .05     .10     .25     .50     .75 
    406   19216      22   12.92       3       3       5      18      19 
    .90     .95 
     21      26 

lowest :  3  4  5  6  7, highest: 25 26 27 28 30 
---------------------------------------------------------------------------
max_yaw_belt 
      n missing  unique 
  19622       0      68 

lowest :      -0.1 -0.2 -0.3 -0.4, highest: 5.6  5.7  6.5  7.0  7.5  
---------------------------------------------------------------------------

```r
# can review more contents of a.
```

To remove the variables with a large number of NAs, I first created a function to identify the indices for the variables with NAs. 
The variables containing NAs, "#DIV/0!" and empty cells should be removed. In addition, the first 6 column variables which are not dirrectly corresponding to the exercise manner would also be removed. Total 54 of variables were remained in the data frame.    

```r
d_na.rm<-d
d_na.rm<-d_na.rm[,-c(1:6, 12:16, 17:19, 21, 22, 24, 25, 26, 27, 28:30, 31:36, 50:59, 
		69:74, 75:80, 81:83, 87, 90, 92,93, 94, 96, 97, 99, 100, 101, 103:112, 
		125, 126, 128, 129, 131, 132, 133, 	134, 135, 136, 137, 138, 139,141:150, 89, 127, 130, 20, 23, 88, 91, 95, 98)]   
dim(d_na.rm) 
```

```
## [1] 19622    54
```


##Splitting the Data  

```r
library(caret)
set.seed(33833)
inTrain<-createDataPartition(d_na.rm$classe, p=3/4, list=FALSE) 
training<-d_na.rm[inTrain,]
testing<-d_na.rm[-inTrain,]
```

##Plotting Predictors    
I used 'car' package to make scatter plot on outcome variable "classe" with multiple pairs of predictors. Here shows an example, the plot shows relationship of "classe" with a few predictors on "belt" which suggested some potentials for good predictors.    

```r
library(car)
scatterplotMatrix(~classe+roll_belt+pitch_belt+yaw_belt+total_accel_belt, span=0.7, data=training) 
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 

```r
dev.copy(png, file="belt.scatterplot.pairs_1.png")
```

png 
  3 

```r
dev.off()
```

pdf 
  2 

##PreProcess using the caret package  
After cleaning the data, there are 54 variables remained. However, some of those variables may be correlated. I used principle componenent analysis to preProcess the data. The results showed that 26 componenets can capture 95% of the variance.  

```r
preProc<-preProcess(training[,-54], method="pca") #26 components to capture 95% of the variance.
preProc
```

```
## 
## Call:
## preProcess.default(x = training[, -54], method = "pca")
## 
## Created from 14718 samples and 53 variables
## Pre-processing: principal component signal extraction, scaled, centered 
## 
## PCA needed 26 components to capture 95 percent of the variance
```

```r
trainPC<-predict(preProc, training[,-54]) 
```

##To train the model using the pre-processed training data.  
The varImp() function shows the most important predictors in the fitted model.    
I chose the random-forest approach which uses bootstrapping re-sampling, and at each split, bootstrpping variables to grow multiple trees to vote, therefore increases the accuracy.   

```r
set.seed(125) 
modelFit<-train(training$classe~., method="rf", data=trainPC)
```

```
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
```

```r
varImp(modelFit)
```

```
## rf variable importance
## 
##   only 20 most important variables shown (out of 26)
## 
##      Overall
## PC8    100.0
## PC15    85.4
## PC1     81.5
## PC13    72.0
## PC12    71.1
## PC3     70.7
## PC5     65.6
## PC9     62.3
## PC2     53.3
## PC6     51.2
## PC16    41.0
## PC10    37.1
## PC18    35.3
## PC7     35.0
## PC4     34.4
## PC17    34.1
## PC14    33.2
## PC21    28.2
## PC23    27.8
## PC22    26.8
```

##Cross validation.
For cross-validation, pre-processing the splitted testing data using the same parameters and method used on the training data. Then make a prediction on the pre-processed testing data and compute the expected out of sample errors. The result shows that the accuracy is 98.2% which is high.

```r
testPC<-predict(preProc, testing[,-54]) 
confusionMatrix(testing$classe, predict(modelFit, testPC)) 
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1386    6    2    1    0
##          B   10  930    8    0    1
##          C    3   12  834    6    0
##          D    0    0   29  775    0
##          E    0    2    5    3  891
## 
## Overall Statistics
##                                         
##                Accuracy : 0.982         
##                  95% CI : (0.978, 0.986)
##     No Information Rate : 0.285         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.977         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.991    0.979    0.950    0.987    0.999
## Specificity             0.997    0.995    0.995    0.993    0.998
## Pos Pred Value          0.994    0.980    0.975    0.964    0.989
## Neg Pred Value          0.996    0.995    0.989    0.998    1.000
## Prevalence              0.285    0.194    0.179    0.160    0.182
## Detection Rate          0.283    0.190    0.170    0.158    0.182
## Detection Prevalence    0.284    0.194    0.174    0.164    0.184
## Balanced Accuracy       0.994    0.987    0.972    0.990    0.998
```

##Fit the model without PCA pre-process.
I also fit the model using the random-forest approach on the training data without PCA preprocess to compare the two approached with or without PCA.  
The result shows that without PCA preprocess, the accuracy is 99.7%, only slightly increases. This suggested that PCA approach can capture the majority of the variance and is effective in this study.  

```r
modelFit2<-train(training$classe~., method="rf", ntree=20, data=training)
```

```
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
## Warning: argument is not numeric or logical: returning NA
```

```r
varImp(modelFit2)
```

```
## rf variable importance
## 
##   only 20 most important variables shown (out of 53)
## 
##                      Overall
## num_window            100.00
## roll_belt              66.89
## pitch_forearm          41.10
## yaw_belt               35.09
## magnet_dumbbell_z      33.72
## roll_forearm           31.83
## magnet_dumbbell_y      31.50
## pitch_belt             27.22
## roll_dumbbell          14.57
## accel_forearm_x        13.13
## magnet_forearm_z       11.04
## magnet_belt_y          10.93
## accel_dumbbell_y       10.90
## magnet_dumbbell_x       9.44
## accel_dumbbell_z        9.24
## total_accel_dumbbell    9.22
## accel_belt_z            8.54
## magnet_belt_x           8.35
## magnet_belt_z           8.02
## accel_dumbbell_x        6.41
```

```r
confusionMatrix(testing$classe, predict(modelFit2, testing)) 
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1394    1    0    0    0
##          B    4  945    0    0    0
##          C    0    6  849    0    0
##          D    0    0    1  803    0
##          E    0    0    1    2  898
## 
## Overall Statistics
##                                         
##                Accuracy : 0.997         
##                  95% CI : (0.995, 0.998)
##     No Information Rate : 0.285         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.996         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.997    0.993    0.998    0.998    1.000
## Specificity             1.000    0.999    0.999    1.000    0.999
## Pos Pred Value          0.999    0.996    0.993    0.999    0.997
## Neg Pred Value          0.999    0.998    1.000    1.000    1.000
## Prevalence              0.285    0.194    0.174    0.164    0.183
## Detection Rate          0.284    0.193    0.173    0.164    0.183
## Detection Prevalence    0.284    0.194    0.174    0.164    0.184
## Balanced Accuracy       0.998    0.996    0.998    0.999    1.000
```

```r
predict(modelFit2, d2_na.rm)
```

```
## Error: object 'd2_na.rm' not found
```

##To predict newdata, the testing data (named "d2") from the assignment.  
To clean the new testing data, I changed the variable name to exactly the same as the training data, which only replace "problem_id" with "classe".  
And change the type of variable "classe" into factor variable with 5 levels. Furthermore, remove the variables in the same way as in the training data.  
Make a prediction using the two trained models used in above,"modelFit" and "modelFit2" which agrees with each other.  


```r
names(d2[,-160])==names(d[,-160]) # all TRUE
```

```
##   [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [30] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [59] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
##  [88] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [117] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
## [146] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE
```

```r
names(d2)=names(d) 

d2_na.rm<-d2
d2_na.rm<-d2_na.rm[,-c(1:6, 12:16, 17:19, 21, 22, 24, 25, 26, 27, 28:30, 31:36, 50:59, 
		69:74, 75:80, 81:83, 87, 90, 92,93, 94, 96, 97, 99, 100, 101, 103:112, 
		125, 126, 128, 129, 131, 132, 133, 	134, 135, 136, 137, 138, 139,141:150, 89, 127, 130, 20, 23, 88, 91, 95, 98)]   
d2_na.rm$classe<-rep(c("A","B","C","D","E"),4)
d2_na.rm$classe<-as.factor(d2_na.rm$classe)

test2PC<-predict(preProc, d2_na.rm[,-54])	

#predict using the first model (PCA pre-process)
predict(modelFit, test2PC)
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```

```r
#predict using the second model (without PCA pre-process)
predict(modelFit2, d2_na.rm)
```

```
##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E
```





