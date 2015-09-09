##CodeBook and Data Dictionary
#Collection of Raw Data
The original data source is found [here] (https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 
The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
-Triaxial Angular velocity from the gyroscope. 
-A 561-feature vector with time and frequency domain variables. 
-Its activity label. 
-An identifier of the subject who carried out the experiment.

I used the following files: 
-features_info.txt': Shows information about the variables used on the feature vector.
-features.txt: List of all features.
-activity_labels.txt: Links the class labels with their activity name.
-train/X_train.txt: Training set.
-train/y_train.txt': Training labels.
-test/X_test.txt: Test set
-test/y_test.txt: Test labels.
#Creating the Tidy File
The details of the code are provided in the README.md file and are summarized here.
Per the instructions of the project to use only mean and standard deviation variables, I extracted only the variables that had the words “mean()” or “std()” in the text from both the test and training data. 
The features names in the features.txt file were used as the basis for the names of the variables in the new test and training data frames. Some clean-up was performed on the names (such as removing the “()”). I decided not to write out all the words since the variable names would be very long. 
I then merged the test and training data. I replaced the integer codes for the activity names with the actual activity names. 
We were then asked to create a new data set with the averages of the variables by Subject and Activity. I melted the data by subject and activity and used dcast to compute the mean and put back into wide format.
The data was written to a file using:
write.table(meantable,"tidydata.txt",row.name=TRUE, quote=FALSE, eol = "\r\n")
You can read the file back in with this command:
table <- read.table("tidydata.txt")
#Description of Variables
Subject (integer)
Number between 1 and 30 representing the subject who carried out the experiment
Activity	(factors)
WALKING_UPSTAIRS
WALKING_DOWNSTAIRS
SITTING
STANDING
LAYING

The remaining variables are all the averages of the respective set of sensor measurement features means and standard deviations taken for each subject during each activity and are normalized and bounded within [-1,1]


 tBodyAcc-mean-X
 tBodyAcc-mean-Y
 tBodyAcc-mean-Z
 tBodyAcc-std-X
 tBodyAcc-std-Y
 tBodyAcc-std-Z
 tGravityAcc-mean-X
 tGravityAcc-mean-Y
 tGravityAcc-mean-Z
 tGravityAcc-std-X
 tGravityAcc-std-Y
 tGravityAcc-std-Z
 tBodyAccJerk-mean-X
 tBodyAccJerk-mean-Y
 tBodyAccJerk-mean-Z
 tBodyAccJerk-std-X
 tBodyAccJerk-std-Y
 tBodyAccJerk-std-Z
 tBodyGyro-mean-X
 tBodyGyro-mean-Y
 tBodyGyro-mean-Z
 tBodyGyro-std-X
 tBodyGyro-std-Y
 tBodyGyro-std-Z
 tBodyGyroJerk-mean-X
 tBodyGyroJerk-mean-Y
 tBodyGyroJerk-mean-Z
 tBodyGyroJerk-std-X
 tBodyGyroJerk-std-Y
 tBodyGyroJerk-std-Z
 tBodyAccMag-mean
 tBodyAccMag-std
 tGravityAccMag-mean
 tGravityAccMag-std
 tBodyAccJerkMag-mean
 tBodyAccJerkMag-std
 tBodyGyroMag-mean
 tBodyGyroMag-std
 tBodyGyroJerkMag-mean
 tBodyGyroJerkMag-std
 fBodyAcc-mean-X
 fBodyAcc-mean-Y
 fBodyAcc-mean-Z
 fBodyAcc-std-X
 fBodyAcc-std-Y
 fBodyAcc-std-Z
 fBodyAccJerk-mean-X
 fBodyAccJerk-mean-Y
 fBodyAccJerk-mean-Z
 fBodyAccJerk-std-X
 fBodyAccJerk-std-Y
 fBodyAccJerk-std-Z
 fBodyGyro-mean-X
 fBodyGyro-mean-Y
 fBodyGyro-mean-Z
 fBodyGyro-std-X
 fBodyGyro-std-Y
 fBodyGyro-std-Z
 fBodyAccMag-mean
 fBodyAccMag-std
 fBodyBodyAccJerkMag-mean
 fBodyBodyAccJerkMag-std
 fBodyBodyGyroMag-mean
 fBodyBodyGyroMag-std
 fBodyBodyGyroJerkMag-mean
 fBodyBodyGyroJerkMag-std
