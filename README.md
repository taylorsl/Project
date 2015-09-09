# Project
Getting and Cleaning Data Project
These are the instructions for the Project associated with Getting and Cleaning Data course. 
The source data is in this [file]
(https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip ).

The file was downloaded and unzipped to a local directory.
The source files should be in the subdirectory in your R working directory: UCI HAR Dataset

##Code executes the following:
1 Merges the training and the test sets to create one data set.
2 Extracts only the measurements on the mean and standard deviation for each measurement. 
3 Uses descriptive activity names to name the activities in the data set
4 Appropriately labels the data set with descriptive variable names. 
5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

 ```
#Libraries required
library(stringr)  
library(dplyr)
library(reshape2)

# General files containing the names of the features to correspond to the columns
# in the X_train.txt and X_test.txt and the activities 
# Assume that files have been zipped into the subdirectory "UCI HAR Dataset"

# Read in General Files
features_file <- "./UCI HAR Dataset/features.txt"
activity_file <- "./UCI HAR Dataset/activity_labels.txt"
features <- read.table(features_file,stringsAsFactors = FALSE)
activity_labels <- read.table(activity_file)

#read in Training Data
Xtrain_file <- "./UCI HAR Dataset/train/X_train.txt"
Ytrain_file <- "./UCI HAR Dataset/train/Y_train.txt"
subtrain_file <- "./UCI HAR Dataset/train/subject_train.txt"
X_train <- read.table(Xtrain_file)
Y_train <- read.table(Ytrain_file)
subjects_train <- read.table(subtrain_file)

#Read in Test Data
Xtest_file <- "./UCI HAR Dataset/test/X_test.txt"
Ytest_file <- "./UCI HAR Dataset/test/Y_test.txt"
subtest_file <- "./UCI HAR Dataset/test/subject_test.txt"
X_test <- read.table(Xtest_file)
Y_test <- read.table(Ytest_file)
subjects_test <- read.table(subtest_file)

# x and y contain row values for the mean and std features
# these will correspond to the column values we want to extract from the test and training data
# will look for items that end in "mean()" or "std()"; other items with the word mean
# are frequency variables
x <- grep("mean()",features[,2],value=FALSE,fixed = TRUE)
y <- grep("std()",features[,2],value=FALSE,fixed = TRUE)
columns <- c(x,y)
columns <- sort(columns)   # sort the combined row values so they are in ascending order

# Extract the meaningful header names from the features vector using features[columns,2]
# Prepend with the headers for the subjects and activity
headers <- c("subject", "activity", features[columns,2])
# Clean up the feature headers removing the "()" in some of the names
headers <- sub("()","",headers,fixed=TRUE)                   

#extract the designated feature columns from X_train and X_test data sets while binding
#using the column variable from above
merged_train <- cbind(subjects_train,Y_train,X_train[,columns]) #Merge the Training data
merged_test <- cbind (subjects_test,Y_test,X_test[,columns])  #Merge the Training data


#merge both training and test set together
#Set the headers to the meaningful names
merged <- rbind(merged_train,merged_test)
names(merged) <- headers  

#Replace the numerical values for the activity names with the names of the activities
for (n in 1:nrow(activity_labels)){
    c <- as.character(n)
    label <- as.character(activity_labels[n,2])
    merged$activity <- str_replace(merged$activity,as.character(n),as.character(activity_labels[n,2]))
}

#cleanup some large objects in memory
rm(X_train)
rm(Y_train)
rm(merged_train)
rm(merged_test)

# Convert the merged data set to long form to more easily compute the means
# Melt the data then
# Recast into wide form using the mean function
melt <- melt(merged,id.vars = c("subject","activity"))
meantable <- dcast (melt, subject+activity~variable,mean)

#Write tidy data in wide form to a txt file
write.table(meantable,"tidydata.txt",row.name=TRUE, quote=FALSE, eol = "\r\n")

#Can read the file back in with the following: table <- read.table("tidydata.txt")

```

