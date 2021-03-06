---
title: "CodeBook.md"
author: "GTolen"
date: "February 29, 2016"
output: 
  html_document:
    highlight: tango
    theme: united
---

# CodeBook.MD for Getting and Cleaning Data Project

##### OBJECTIVES of the R Script, run_analysis.R:
1. Merge the training and the test sets to create one data set.
1. Extract only measurements on the mean and standard deviation for each measurement.
1. Use descriptive activity names to name activities in the dataset   
1. Label the data set with descriptive variable names
1. Create a second, independent tidy data set with the average of each variable for each activity and each subject after (1) to (4).  Thus, the average will be calculated from Columns 2 to 34.

##### DATA
* Download the file from the Source and save the files to the working directory: [https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip](link path)
* Description is available at:                                                                                         [http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones](link path)

* The following files are included in the downloaded Source zip file:
1.  features_info.txt
     -includes description of the variables listed in features.txt which are the columns
     of the X_train.txt and X_test.txt data sets
1.  features.txt
     -list of the columns included in the X_train and X_test files
1.  activity_labels.txt
     -the code for the six activities each of the 30 individuals were asked to perform
      1 - Walking; 2 - Walking upstairs; 3 - Walking downstairs; 4 - Sitting;
      5 - Standing; 6 - Laying
1.  README.txt
     -describes how the experiment and how the data was collected
     -includes a list of the files included in the dataset, notes and license
1.  from train folder:
     - 5-a. X_train.txt
     - 5-b. y_train.txt
     - 5-c. subject_train.txt
     - X_train.txt consists of 7352R x 561C;
     - columns are the variables estimated from the signals and those obtained by
     "averaging the signals in a signal window example" as described in the
     features_info.txt
     - Y_train.txt consists of 7352R; values are the activity code for the X_train data
     - subject_train.txt relates each row of the X_train data with the subject who
     performed the activity
     - values are 1, 3, 5, 6, 7, 8, 11, 14, 15, 16, 17, 19, 21, 22, 23, 25, 26, 27, 28, 29, 30
1.  from test folder:
     - 6-a. X_test.txt
     - 6-b. y_test.txt
     - 6-c. subject_test.txt
     - X_test.txt consists of 2947R x 561C;
     - columns are the variables estimated from the signals and those obtained by
     "averaging the signals in a signal window example" as described in the
     features_info.txt
     - Y_test.txt consists of 2947R; values are the activity code for the X_train data
     - subject_train.txt relates each row of the X_test data with the subject who
     performed the activity
     - values are 2, 4, 9, 10, 12, 13, 18, 20, 24
1.  from train/Inertial Signals folder
     - 7.a. body_acc_x_train.txt
     - 7.b. body_acc_y_train.txt
     - 7.c. body_acc_z_train.txt
     - 7.d. body_gyro_x_train.txt
     - 7.e. body_gyro_y_train.txt
     - 7.f  body_gyro_z_train.txt
     - 7.g. total_acc_x_train.txt
     - 7.h. total_acc_y_train.txt
     - 7.i. total_acc_z_train.txt
1.  from test/Inertial Signals folder
     - 8.a. body_acc_x_test.txt
     - 8.b. body_acc_y_test.txt
     - 8.c. body_acc_z_test.txt
     - 8.d. body_gyro_x_test.txt
     - 8.e. body_gyro_y_test.txt
     - 8.f  body_gyro_z_test.txt
     - 8.g. total_acc_x_test.txt
     - 8.h. total_acc_y_test.txt
     - 8.i. total_acc_z_test.txt
     - files from (7) and (8) were not directly used for the run_analysis.R program
     - information have been used to derive the values available in the X_train and X_test files
     as described in the features_info.txt

##### DESCRIPTION OF INFORMATION AVAILABLE TAKEN FROM features_info.txt and ReadMe.txt
* (For quick reference, this is a short description of the information available TAKEN FROM FEATURES_INFO.TXT AND README.TXT.  Please see features_info.txt and ReadMe.txt included in the downloaded zip file for more details.)

The features/measurements were sourced from the accelerometer and gyroscope 3-axial signals tAcc-XYZ and tGyro-XYZ.  The acceleration signal was separated into body and gravity acceleration signals, tBodyAcc-XYZ and tGravityAcc-XYZ.  Furthermore, they were able to derive the Jerk signals, tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ using the body linear acceleration and angular velocity from the experiement.  Magnitude of the signals were derived using Euclidean norm
(tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBdoyGyroMag and tBodyGyroJerkMag).  A Fast Fourier Transformation was applied to some of these signals to derive values for fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ,  fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag.The -XYZ indicates the signals in the X, Y and Z directions.  The experiment made use of 128 readings.

The following data, corresponding to columns of the X_train.txt and X_test.txt dataset files, were made available based from the measurements derived from the experiment (suffix/measurement):

1.    mean(): mean
1.  std(): standard deviation
1.  mad(): median absolute deviation
1.  max(): largest value in array
1.  min(): smallest value in array
1.  sma(): signal magnitude area
1.  energy(): energy measure
1.  iqr(): interquartile range
1.  entropy(): signal entropy
1. arCoeff(): autoregression coefficients, Burg order = 4
1. correlation(): correlation coefficient between two signals
1. maxinds(): index of the frequency component with largest magnitude
1. meanFreq(): weighted average of the frequency components to obtain a mean frequency
1. skewness(): skewness of the frequency domain signal
1. kurtosis(): kurtosis of the frequency domain signal
1. bandsEnergy(): energy of a frequency interval within the 64 bins of the FFT of each window
1. angle(): Angle between to vectors

Some variations (example, measurements with respect to the axis) were also obtained. Please see features_info.txt for the details.

*For the Course Project, we are only interested on the -mean() and -std() columns.*

##### THE RUN_ANALYSIS.R R SCRIPT
Requires the following libraries:

1. dplyr
1. MASS

Information from the input data files were stored in the following variables:

1.  features<-read.table("features.txt")
1.  act_labels<-read.table("activity_labels.txt")
1.  y_train<-read.table("y_train.txt")
1.  y_test<-read.table("y_test.txt")
1.  subject_train<-read.table("subject_train.txt")
1.  subject_test<-read.table("subject_test.txt")
1.  x_train<-read.table("X_train.txt",col.names=features$V2)
1.  x_test<-read.table("X_test.txt",col.names=features$V2)
    * Information from subject_train and y_train is merged with the x_train variable.
    * Similarly, information from subject_test and y_test is merged with the x_test variable.The appended columns were renamed subject_id and activity.
1.  SubFeatures variable captures the column numbers of column/variable names with -mean() or - std() sinc these are the only columns for the aggregation, and includes the subject_id and activity columns for the subgrouping.
1.  AllData merges the rows from x_train and x_test, thus, combining the training and the test data.  The descriptive variable name for the activity is also added to the data frame.          
1. TidySet calculates the average of each column for each subject individual - activity pair.  
    * The final text output file, "Output_Mean_Grouped.txt", is based on this data frame.
    * The subject individual data is stored in AllData$subject_id.
    * The activity data is stored in AllData$desc_activity.


OUTPUT: VARIABLES / COLUMNS OF THE TIDYSET DATA FRAME

1. These are the output columns of the data frame *TidySet* generated by run_analysis.R.
    * The information from the TidySet data frame is exported to *"Output_Mean_Grouped.txt"*
with " " delimiter.
1. Group.1 and Group.2 indicate which subset the mean value from each of the columns is associated to.
    * Group.1 corresponds to one of the 30 subject individuals.
    * Group.2  corresponds to one of the activities performed by the subject individuals.
    * Both columns are of *character* class
1.  Columns/Variables listed below are the averages of the values of the -mean() and -std() columns for each group:

    1.  tBodyAcc.mean...X          
    1.  tBodyAcc.mean...Y          
    1.  tBodyAcc.mean...Z          
    1.  tBodyAcc.std...X           
    1.  tBodyAcc.std...Y           
    1.  tBodyAcc.std...Z           
    1.  tGravityAcc.mean...X       
    1.  tGravityAcc.mean...Y       
    1.  tGravityAcc.mean...Z       
    1.  tGravityAcc.std...X        
    1.  tGravityAcc.std...Y        
    1.  tGravityAcc.std...Z        
    1.  tBodyAccJerk.mean...X      
    1.  tBodyAccJerk.mean...Y      
    1.  tBodyAccJerk.mean...Z      
    1.  tBodyAccJerk.std...X       
    1.  tBodyAccJerk.std...Y       
    1.  tBodyAccJerk.std...Z       
    1.  tBodyGyro.mean...X         
    1.  tBodyGyro.mean...Y         
    1.  tBodyGyro.mean...Z         
    1.  tBodyGyro.std...X          
    1.  tBodyGyro.std...Y          
    1.  tBodyGyro.std...Z          
    1.  tBodyGyroJerk.mean...X     
    1.  tBodyGyroJerk.mean...Y     
    1.  tBodyGyroJerk.mean...Z     
    1.  tBodyGyroJerk.std...X      
    1.  tBodyGyroJerk.std...Y      
    1.  tBodyGyroJerk.std...Z      
    1.  tBodyAccMag.mean..         
    1.  tBodyAccMag.std..          
    1.  tGravityAccMag.mean..      
    1.  tGravityAccMag.std..       
    1.  tBodyAccJerkMag.mean..     
    1.  tBodyAccJerkMag.std..      
    1.  tBodyGyroMag.mean..        
    1.  tBodyGyroMag.std..         
    1.  tBodyGyroJerkMag.mean..    
    1.  tBodyGyroJerkMag.std..     
    1.  fBodyAcc.mean...X          
    1.  fBodyAcc.mean...Y          
    1.  fBodyAcc.mean...Z          
    1.  fBodyAcc.std...X           
    1.  fBodyAcc.std...Y           
    1.  fBodyAcc.std...Z           
    1.  fBodyAccJerk.mean...X      
    1.  fBodyAccJerk.mean...Y      
    1.  fBodyAccJerk.mean...Z      
    1.  fBodyAccJerk.std...X       
    1.  fBodyAccJerk.std...Y       
    1.  fBodyAccJerk.std...Z       
    1.  fBodyGyro.mean...X         
    1.  fBodyGyro.mean...Y         
    1.  fBodyGyro.mean...Z         
    1.  fBodyGyro.std...X          
    1.  fBodyGyro.std...Y          
    1.  fBodyGyro.std...Z          
    1.  fBodyAccMag.mean..         
    1.  fBodyAccMag.std..          
    1.  fBodyBodyAccJerkMag.mean..
    1.  fBodyBodyAccJerkMag.std..  
    1.  fBodyBodyGyroMag.mean..    
    1.  fBodyBodyGyroMag.std..     
    1.  fBodyBodyGyroJerkMag.mean..
    1.  fBodyBodyGyroJerkMag.std..

* All of these columns are of *numeric class*.

##### OTHER NOTES:
* More information on the run_analysis.R script is available in the README.md file.
