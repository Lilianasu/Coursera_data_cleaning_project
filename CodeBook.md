## Structure of the 'finaltidydataset.csv'
This dataframe is composed by 68 variables (columns) and 180 observations (rows)

From the 68 variables, 2 are ID variables (activity & subject) and 66 are measurement variables.

The values of the observations are the average of each measurement variable for each activity and each subject.


## ID variables

####activity
Factor w/ 6 levels  
Activity the subject was doing while he/she was being measured
	
		walking 
		walking_upstairs 
		walking_downstairs 
		sitting 
		standing 
		laying
	
####subject
Factor w/ 30 levels
Number of subject
		
		values 1 to 30

		
## Measurements variables

The 66 measurement variables are the following
```
tBodyAcc-mean()-X
tBodyAcc-mean()-Y
tBodyAcc-mean()-Z
tBodyAcc-std()-X
tBodyAcc-std()-Y
tBodyAcc-std()-Z
tGravityAcc-mean()-X
tGravityAcc-mean()-Y
tGravityAcc-mean()-Z
tGravityAcc-std()-X
tGravityAcc-std()-Y
tGravityAcc-std()-Z
tBodyAccJerk-mean()-X
tBodyAccJerk-mean()-Y
tBodyAccJerk-mean()-Z
tBodyAccJerk-std()-X
tBodyAccJerk-std()-Y
tBodyAccJerk-std()-Z
tBodyGyro-mean()-X
tBodyGyro-mean()-Y
tBodyGyro-mean()-Z
tBodyGyro-std()-X
tBodyGyro-std()-Y
tBodyGyro-std()-Z
tBodyGyroJerk-mean()-X
tBodyGyroJerk-mean()-Y
tBodyGyroJerk-mean()-Z
tBodyGyroJerk-std()-X
tBodyGyroJerk-std()-Y
tBodyGyroJerk-std()-Z
tBodyAccMag-mean()
tBodyAccMag-std()
tGravityAccMag-mean()
tGravityAccMag-std()
tBodyAccJerkMag-mean()
tBodyAccJerkMag-std()    
tBodyGyroMag-mean()
tBodyGyroMag-std()
tBodyGyroJerkMag-mean()
tBodyGyroJerkMag-std()
fBodyAcc-mean()-X
fBodyAcc-mean()-Y         
fBodyAcc-mean()-Z
fBodyAcc-std()-X
fBodyAcc-std()-Y          
fBodyAcc-std()-Z
fBodyAccJerk-mean()-X
fBodyAccJerk-mean()-Y     
fBodyAccJerk-mean()-Z
fBodyAccJerk-std()-X
fBodyAccJerk-std()-Y      
fBodyAccJerk-std()-Z
fBodyGyro-mean()-X
fBodyGyro-mean()-Y        
fBodyGyro-mean()-Z
fBodyGyro-std()-X
fBodyGyro-std()-Y         
fBodyGyro-std()-Z
fBodyAccMag-mean()
fBodyAccMag-std()
fBodyBodyAccJerkMag-mean()
fBodyBodyAccJerkMag-std()
fBodyBodyGyroMag-mean()
fBodyBodyGyroMag-std()
fBodyBodyGyroJerkMag-mean()
fBodyBodyGyroJerkMag-std()
```

## Description of abbreviations of measurements

leading t is for time, or f is for frequency measurements.

Body = related to body movement.

Gravity = acceleration of gravity

Acc = accelerometer measurement

Gyro = gyroscopic measurements

Jerk = sudden movement acceleration

Mag = magnitude of movement

The units given are g’s for the accelerometer and rad/sec for the gyro and g/sec and rad/sec/sec for the corresponding jerks.

These signals were used to estimate variables of the feature vector for each pattern:
‘-XYZ’ is used to denote 3-axial signals in the X, Y and Z directions. They total 33 measurements including the 3 dimensions - the X,Y, and Z axes.
```
tBodyAcc-XYZ
tGravityAcc-XYZ
tBodyAccJerk-XYZ
tBodyGyro-XYZ
tBodyGyroJerk-XYZ
tBodyAccMag
tGravityAccMag
tBodyAccJerkMag
tBodyGyroMag
tBodyGyroJerkMag
fBodyAcc-XYZ
fBodyAccJerk-XYZ
fBodyGyro-XYZ
fBodyAccMag
fBodyAccJerkMag
fBodyGyroMag
fBodyGyroJerkMag
```
The set of variables that were estimated from these signals are:

```
mean(): Mean value
std(): Standard deviation
```


## Description of the script. Transformation of the raw data into the clean data  :green_apple:
### 0. Getting the land ready  
0.1 Install packages and libraries
```
install.packages("downloader")
library(downloader)
library(dplyr)    # Data frames tool
library(reshape2) # Help with melt and cast of data frames
```
0.2 Set working directory. Download and unzip files in "wearable" folder.

A datacleaningfinalproject folder is created, and is set as the working directory. This is where the raw data is downloaded.
```
if(!exists("~/datacleaningfinalproject")){dir.create("~/datacleaningfinalproject")}
setwd("~/datacleaningfinalproject")
myurl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download(url = myurl, destfile="./wearable.zip")
unzip("./wearable.zip")
```

### 1. Merge the training and the test sets to create one data set.
1.1 Reading the Test and Train sets.

These two sets are read into dataframes.
```
testset <- read.table("./wearable/test/X_test.txt", stringsAsFactors = F)
trainset <- read.table("./wearable/train/X_train.txt", stringsAsFactors = F)
```
1.2 Binding both sets.

Take into account that first comes Test, second Train
```
testandtrainset <- rbind(testset,trainset)
```
Getting the names of the features columns, and the mask for the mean and standard deviation data.

The names of the columns are read in order to filter only those which are mean or standard deviation related.
```
features <- read.table("./wearable/features.txt", stringsAsFactors = F)
featureslist <- features[,2]
featuresmask <- grep("mean\\(\\)|std\\(\\)", featureslist)
featureslist <- featureslist[featuresmask]
```
### 2. Extracts only the measurements on the mean and standard deviation for each measurement.
2.1 Delimiting the testandtrainset set to the mean and standard deviation data
```
testandtrainset <- select(testandtrainset, featuresmask)
```
### 3. Uses descriptive activity names to name the activities in the data set
3.1 Adding descriptive names to the columns.

The names are taken from wearable\features.txt
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
### 4.Appropriately labels the data set with descriptive variable names.
4.1 First comes Test, second Train
```
activities <- rbind(testactivities,trainactivities)
subjects <- rbind(testsubjects,trainsubjects)
testandtrainset <- mutate(testandtrainset, activity=activities[,1], subject=subjects[,1])
```
### 5. From the data set in step 4, creates a second, independent tidy data set with 
5.1 The average of each variable for each activity and each subject.
```
ttmolten <- melt(testandtrainset, id=c("activity", "subject"), na.rm = TRUE)
finaltable <- dcast(ttmolten, formula = activity + subject ~ variable, mean)
```
5.2 Save data into .csv file
```
write.table(finaltable, file="finaltidydataset.txt", row.names = F)
```
