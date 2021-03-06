# Project 1: Predict the Housing Prices in Ames #

### Spring 2021 ###

## Data set ##

Download the dataset, Ames_data.csv, from the Resouces page on Piazza. The dataset has 2930 rows (i.e., houses) and 83 columns.

* The first column is “PID”, the Parcel identification number;
* The last column is the response variable, Sale_Price;
* The remaining 81 columns are explanatory variables describing (almost) every aspect of residential homes.
  Test IDs
  
Download project1_testIDs.dat from the Resouces page on Piazza. This file contains 879 rows and 10 columns, which will be used to generate 10 sets of training/test splits from Ames_data.csv. Each column contains the 879 row-numbers of a test data.

Here is how you generate a split of training and test using the j-th column of project1_testIDs.dat in R.

```
data <- read.csv("Ames_data.csv")
testIDs <- read.table("project1_testIDs.dat")
j <- 2
train <- data[-testIDs[,j], ]
test <- data[testIDs[,j], ]
test.y <- test[, c(1, 83)]
test <- test[, -83]
write.csv(train,"train.csv",row.names=FALSE)
write.csv(test, "test.csv",row.names=FALSE)
write.csv(test.y,"test_y.csv",row.names=FALSE)

```
Save the training and test as csv files. In particular, the test data are saved as two seperate two files: one containing just the feature vectors and the other one containing the response column.

For your remaining analysis, you need to read the training and test data from the csv files.

```
train <- read.csv("train.csv")
test <- read.csv("test.csv")
```

## Goal ##

The goal is to predict the final price of a home (in log scale) with those explanatory variables. You need to build Two prediction models selected from the following three:

* one based on linear regression models with Lasso or Ridge or Elasticnet penalty;
* one based on tree models, such as randomForest or boosting tree;
* The features for the two models do not need to be the same. Please check Piazza for packages students are allowed to use for this project.

## Source ##

De Cock, D. (2011). “Ames, Iowa: Alternative to the Boston Housing Data as an End of Semester Regression Project,” Journal of Statistics Education, Volume 19, Number 3. [PDF](http://ww2.amstat.org/publications/jse/v19n3/decock.pdf "PDF Title")

Check variable description [Here](https://ww2.amstat.org/publications/jse/v19n3/decock/DataDocumentation.txt "Here Title")

This data set has been used in a Kaggle competition (https://www.kaggle.com/c/house-prices-advanced-regression-techniques). You can check how others analyze this data and try some sample code on Kaggle. Note that our data set has two more explanatory variables, “Longitude” and “Latitude.”

## What to Submit ##

Submit the following two items on Coursera:

* R/Python code in a single file named mymain.R (or mymain.py) that takes a training data and a test data as input and outputs two submission files (the format of the submission file is given below). No zip file; no R markdown file.
 The structure of your mymain.R (or mymain.py) should look like the following. Note that you have to pre-process your training and test data separately.

```
# Step 0: Load necessary libraries
###########################################
# Step 0: Load necessary libraries
#
# YOUR CODE
# 


###########################################
# Step 1: Preprocess training data
#         and fit two models
#
train <- read.csv("train.csv")
#
# YOUR CODE
# 


###########################################
# Step 2: Preprocess test data
#         and output predictions into two files
#
test <- read.csv("test.csv")
#
# YOUR CODE
# 
```

## A report (3 pages maximum, PDF or HTML) that provides: ##

* technical details ( e.g., pre-processing, implementation details if not trivial) for the models you use, and
* any interesting findings from your analysis.
* In addition, report the accuracy on the test data (see evaluation metric given below), running time of your code and the computer system you use (e.g., Macbook Pro, 2.53 GHz, 4GB memory, or AWS t2.large) for the 10 training/test splits. You do not need to submit the part of the code that evaluates test accuracy.

In your report, describe the techincal details of your code, summarize your findings, and do not copy-and-paste your code to the report.

## Code Evaluation ##

We will run the command “source(mymain.R)” in a directory, in which there are only two files: train.csv and test.csv, one of the 10 training/test splits.

* train.csv: 83 columns;
* test.csv: 82 columns without the column “Sale_Price”.

After running your code, we should see Two txt files in the same directory named “mysubmission1.txt” and “mysubmission2.txt.” Each submission file contains prediction on the test data from a model.

Submission File Format. The file should have the following format (do not forget the comma between PID and Sale_Price):

PID,  | Sale_Price
------------- | -------------
528221060,  | 169000.7
535152150,  | 14523.6
533130020, | 195608.2 



__Evaluation Metric:__

Submission are evaluated on Root-Mean-Squared-Error (RMSE) between the logarithm of the predicted price and the logarithm of the observed sales price. Our evaluation R code looks like the following:

```
# Suppose we have already read test_y.csv in as a two-column 
# data frame named "test.y":
# col 1: PID
# col 2: Sale_Price

pred <- read.csv("mysubmission1.txt")
names(test.y)[2] <- "True_Sale_Price"
pred <- merge(pred, test.y, by="PID")
sqrt(mean((log(pred$Sale_Price) - log(pred$True_Sale_Price))^2))
```

__Performance Target:__ 

Your performance is based on the minimal RMSE from the two models. Full credit for submissions with minimal RMSE less than

* 0.125 for the first 5 training/test splits and
* 0.135 for the remaining 5 training/test splits.
