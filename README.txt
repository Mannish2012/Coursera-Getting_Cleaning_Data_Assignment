Getting and Cleaning Data Course Project


The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following.

Merges the training and the test sets to create one data set.
Extracts only the measurements on the mean and standard deviation for each measurement.
Uses descriptive activity names to name the activities in the data set
Appropriately labels the data set with descriptive variable names.
From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

The Data

Now that we've seen the instructions, let's review the data. 

Let's first look at the X_train.txt data set. In X_train.txt, there are 7352 rows and 561 columns. In the 'train' set, there are 2947 rows and 561 columns. Therefore, there are a total of 10,299 rows in the two datasets. 

So, when we merge the two datasets, we get a matrix with 10,299 rows and 561 columns. 

Now, each of these 561 columns are a particular attribute. If we look at the features.txt file, we notice that the first 6 columns are 

1 tBodyAcc-mean()-X


2 tBodyAcc-mean()-Y


3 tBodyAcc-mean()-Z


4 tBodyAcc-std()-X


5 tBodyAcc-std()-Y


6 tBodyAcc-std()-Z

The first 6 columns are all measurements of means or standard deviations, so we need them. But we need all the columns with the means or standard deviations. From perusing these first columns, we notice that mean is referred to by "mean" and standard deviation by "std", so we will use the grep function to select all the columns with means or standard deviations. 

Let's consider all our data sets: 

features -- We get this by reading in the features.txt data set. It is a 561 X 2 dataset. The first column (V1) is the serial number for the particular feature vector (V2). 

activity_labels -- We get this by reading in the activity_labels.txt data set. It is a 6 X 2 dataset. The first column (V1) ascribes numbers 1-6 to the activities in the second column (V2).

testSet -- We get this by reading in the X_train.txt dataset. It is a 2947 X 561 dataset. Each column of the dataset is a features dataset, as described earlier. Each row of the dataset is an observation. 

trainSet -- We get this in a way similar to testSet, from the X_train.txt dataset. It is a 7352 X 561 column dataset. It is interpreted similarly as testSet. 

mergedSet -- This is the composite dataset we get by binding testSet and trainSet together with rbind(). Thus, the dimensions of this dataset are 10299 X 561. 

testMoves -- We get this by reading in y_test.txt. This is a 2947 X 1 dataset. The rows correspond to the attribute numbers (1-6) of the activities described earlier. In other words, the MOVEMENT during each observation for the TEST 2947 observations. 

trainMoves -- We get this similarly to testMoves by reading in from y_train.txt. It is a 7352 X 1 dataset, and is interpreted simiarly as testMoves.

mergedMoves -- This is the composite dataset we get by binding testMoves and trainMoves together with rbind(). Thus, the dimensions of this dataset are 10299 X 1.

testPerson -- We get this by reading in the subject_test. txt datafile. This is a 2947 X 1 datafile. The rows identify the subjects that carried out the experiment. 

testPerson %>% unique shows us the unique people who were part of this experiment. 
     V1
1     2
303   4
620   9
908  10
1202 12
1522 13
1849 18
2213 20
2567 24

Column V1 shows us the unique subjects. We notice there are 9 subjects. 

trainPerson -- We get this by reading in from subject_train analogously to testPerson. 

trainPerson %>% unique gives us: 

     V1
1     1
348   3
689   5
991   6
1316  7
1624  8
1905 11
2221 14
2544 15
2872 16
3238 17
3606 19
3966 21
4374 22
4695 23
5067 25
5476 26
5868 27
6244 28
6626 29
6970 30

As expected, there are 21 subjects. (Use trainperson %>% unique %>% dim to find out). 

mergedPerson -- We get the composite dataset by using rbind() on testPerson and trainPerson. 

The datasets we need are the merged datasets. We need therefore, mergedSet, mergedMoves and mergedPerson. 

We need to create an independent, tidy dataset with the average of each variable for each activity and each subject. Now: 

> mergedPerson %>% dim gives us 
[1] 10299     1

> mergedPerson %>% unique %>% dim and 
[1] 30  1

So we know there are there are 10,299 total observations in mergedPerson, but only 30 UNIQUE ones. These unique ones correspond to our subjects. Again, we have verified that the mergedPerson is a 1-column dataset that contains the identification data (and ID data only) on ALL of our subjects. We need to combine this with another merged dataset that describes activity. 

> mergedMoves %>% dim
[1] 10299     1

> mergedMoves %>% unique %>% dim
[1] 6 1

They show us that while mergedMoves has 10,299 observations, only 6 of them are unique. These are our activities. Therefore, we can confidently combine mergedMoves to mergedPerson by using the cbind() function. This will bind the columns. 



