# Readme file
Coursera´s Getting and Cleaning Data Course

Final Project

Liliana Hirugame

September, 2016

## Description of the project  :pushpin:

The components of this Course's Final Project are the next

1) finaltidydataset.csv 
  A tidy data set 
2) A link to a Github repository with your script for performing the analysis
3) CodeBook.md
   A code book that describes the variables, the data, and any transformations or work performed to clean up the data. 

The intention of the run_analysis.R script is to prepare tidy data that can be used for later analysis.

The data used is from the site: 
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip


## 0. Get the land ready  :green_apple:
0.1 Install packages and libraries
```
install.packages("downloader")
library(downloader)
library(dplyr)    # Data frames tool
library(reshape2) # Help with melt and cast of data frames
```
0.2 Set working directory. Download and unzip files in "wearable" folder
```
if(!exists("~/datacleaningfinalproject")){dir.create("~/datacleaningfinalproject")}
setwd("~/datacleaningfinalproject")
myurl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download(url = myurl, destfile="./wearable.zip")
unzip("./wearable.zip")
```

## 1. Merge the training and the test sets to create one data set.
1.1 Reading the Test and Train sets

```
testset <- read.table("./wearable/test/X_test.txt", stringsAsFactors = F)
trainset <- read.table("./wearable/train/X_train.txt", stringsAsFactors = F)
```
1.2 Binding both sets
Take into account that first comes Test, second Train
```
testandtrainset <- rbind(testset,trainset)
```
Getting the names of the features columns, and the mask for the mean and standard deviation data
```
features <- read.table("./wearable/features.txt", stringsAsFactors = F)
featureslist <- features[,2]
featuresmask <- grep("mean\\(\\)|std\\(\\)", featureslist)
featureslist <- featureslist[featuresmask]
```
## 2. Extracts only the measurements on the mean and standard deviation for each measurement.
2.1 Delimiting the testandtrainset set to the mean and standard deviation data
```
testandtrainset <- select(testandtrainset, featuresmask)
```
## 3. Uses descriptive activity names to name the activities in the data set
3.1 Adding descriptive names to the columns
```
colnames(testandtrainset) <- featureslist
```
3.2 Reading Test and Train Activities, setting them as factors
```
testactivities <- read.table("./wearable/test/y_test.txt", stringsAsFactors = F)
testactivities <- mutate(testactivities, V1=as.factor(V1) )
trainactivities <- read.table("./wearable/train/y_train.txt", stringsAsFactors = F)
trainactivities <- mutate(trainactivities, V1=as.factor(V1) )
```
3.3 Setting the levels  to define the activities
```
levels(testactivities[,1]) <- c("walking", "walking_upstairs","walking_downstairs","sitting","standing","laying")
levels(trainactivities[,1]) <- c("walking", "walking_upstairs","walking_downstairs","sitting","standing","laying")
```
3.4 Reading Test and Train Subjects, setting them as factors
```
testsubjects <- read.table("./wearable/test/subject_test.txt", stringsAsFactors = F)
testsubjects <- mutate(testsubjects, V1=as.factor(V1) )
trainsubjects <- read.table("./wearable/train/subject_train.txt", stringsAsFactors = F)
trainsubjects <- mutate(trainsubjects, V1=as.factor(V1) )
```
## 4.Appropriately labels the data set with descriptive variable names.
4.1 First comes Test, second Train
```
activities <- rbind(testactivities,trainactivities)
subjects <- rbind(testsubjects,trainsubjects)
testandtrainset <- mutate(testandtrainset, activity=activities[,1], subject=subjects[,1])
```
## 5. From the data set in step 4, creates a second, independent tidy data set with 
5.1 The average of each variable for each activity and each subject.
```
ttmolten <- melt(testandtrainset, id=c("activity", "subject"), na.rm = TRUE)
finaltable <- dcast(ttmolten, formula = activity + subject ~ variable, mean)
```


## References   :books:


Basic writing and formatting syntax on GitHub
https://help.github.com/articles/basic-writing-and-formatting-syntax/
