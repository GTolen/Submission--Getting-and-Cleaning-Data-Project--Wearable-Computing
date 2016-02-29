CodeBook.MD
for Getting and Cleaning Data Project
GNTolentino, 28 Feb 2016

OBJECTIVES of the R Script, run_analysis.R:
1. Merge the training and the test sets to create one data set.
2. Extract only measurements on the mean and standard deviation
   for each measurement.
3. Use descriptive activity names to name activities in the dataset   
4. Label the data set with descriptive variable names
5. Create a second, independent tidy data set with the average
of each variable for each activity and each subject after (1) to (4).
Thus, the mean will be calculated from Columns 2 to 34.

DATA
Download the file from the Source and save the files to the working directory:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
Description is available at:
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

The following files are included in the downloaded Source zip file:
1.  features_info.txt
     -includes description of the variables listed in features.txt which are the columns
     of the X_train.txt and X_test.txt data sets
2.  features.txt
     -list of the columns included in the X_train and X_test files
3.  activity_labels.txt
     -the code for the six activities each of the 30 individuals were asked to perform
      1 - Walking; 2 - Walking upstairs; 3 - Walking downstairs; 4 - Sitting;
      5 - Standing; 6 - Laying
4.  README.txt
     -describes how the experiment and how the data was collected
     -includes a list of the files included in the dataset, notes and license
5.  from train folder:
5.a. X_train.txt
5.b. y_train.txt
5.c. subject_train.txt
     - X_train.txt consists of 7352R x 561C;
     - columns are the variables estimated from the signals and those obtained by
     "averaging the signals in a signal window example" as described in the
     features_info.txt
     - Y_train.txt consists of 7352R; values are the activity code for the X_train data
     - subject_train.txt relates each row of the X_train data with the subject who
     performed the activity
     - values are 1, 3, 5, 6, 7, 8, 11, 14, 15, 16, 17, 19, 21, 22, 23, 25, 26, 27, 28, 29, 30
6.  from test folder:
6.a. X_test.txt
6.b. y_test.txt
6.c. subject_test.txt
     - X_test.txt consists of 2947R x 561C;
     - columns are the variables estimated from the signals and those obtained by
     "averaging the signals in a signal window example" as described in the
     features_info.txt
     - Y_test.txt consists of 2947R; values are the activity code for the X_train data
     - subject_train.txt relates each row of the X_test data with the subject who
     performed the activity
     - values are 2, 4, 9, 10, 12, 13, 18, 20, 24
7.  from train/Inertial Signals folder
7.a. body_acc_x_train.txt
7.b. body_acc_y_train.txt
7.c. body_acc_z_train.txt
7.d. body_gyro_x_train.txt
7.e. body_gyro_y_train.txt
7.f  body_gyro_z_train.txt
7.g. total_acc_x_train.txt
7.h. total_acc_y_train.txt
7.i. total_acc_z_train.txt
8.  from test/Inertial Signals folder
8.a. body_acc_x_test.txt
8.b. body_acc_y_test.txt
8.c. body_acc_z_test.txt
8.d. body_gyro_x_test.txt
8.e. body_gyro_y_test.txt
8.f  body_gyro_z_test.txt
8.g. total_acc_x_test.txt
8.h. total_acc_y_test.txt
8.i. total_acc_z_test.txt
     - files from 7 and 8 were not directly used for the run_analysis.R program
     - information have been used to derive the values in the X_train and X_test files
     as described in the features_info.txt

DESCRIPTION OF INFORMATION AVAILABLE TAKEN FROM features_info.txt and ReadMe.txt
(This is a short description of the information available TAKEN FROM FEATURES_INFO.TXT AND README.TXT.
Please see features_info.txt and ReadMe.txt included in the zip file for more details.)
The features/measurements were sourced from the accelerometer and gyroscope 3-axial signals
tAcc-XYZ and tGyro-XYZ.  The acceleration signal was separated into body and gravity acceleration
signals, tBodyAcc-XYZ and tGravityAcc-XYZ.  Furthermore, they were able to derive the Jerk signals,
tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ using the body linear acceleration and angular velocity
from the experiement.  Magnitude of the signals were derived using Euclidean norm
(tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBdoyGyroMag and tBodyGyroJerkMag).  
A Fast Fourier Transformation was applied to some of these signals to derive values
for fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ,  fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag.
The -XYZ indicates the signals in the X, Y and Z directions.  The experiment made use of 128 readings.

The following data, corresponding to columns of the X_train.txt
and X_test.txt dataset files, were made available based from the measurements derived
from the experiment (suffix/measurement):
1.  mean(): mean
2.  std(): standard deviation
3.  mad(): median absolute deviation
4.  max(): largest value in array
5.  min(): smallest value in array
6.  sma(): signal magnitude area
7.  energy(): energy measure
8.  iqr(): interquartile range
9.  entropy(): signal entropy
10. arCoeff(): autoregression coefficients, Burg order = 4
11. correlation(): correlation coefficient between two signals
12. maxinds(): index of the frequency component with largest magnitude
13. meanFreq(): weighted average of the frequency components to obtain a mean frequency
14. skewness(): skewness of the frequency domain signal
15. kurtosis(): kurtosis of the frequency domain signal
16. bandsEnergy(): energy of a frequency interval within the 64 bins of the FFT of each window
17. angle(): Angle between to vectors
Some variations (example, measurements with respect to the axis) were also obtained.
Please see features_info.txt for the details.

For the Course Project, we are only interested on the -mean() and -std() columns.


THE RUN_ANALYSIS.R R SCRIPT
Requires the following libraries:
1. dplyr
2. MASS

Information from the input data files were stored in the following variables:
1.  features<-read.table("features.txt")
2.  act_labels<-read.table("activity_labels.txt")
3.  y_train<-read.table("y_train.txt")
4.  y_test<-read.table("y_test.txt")
5.  subject_train<-read.table("subject_train.txt")
6.  subject_test<-read.table("subject_test.txt")
7.  x_train<-read.table("X_train.txt",col.names=features$V2)
8.  x_test<-read.table("X_test.txt",col.names=features$V2)
      Information from subject_train and y_train is merged with the x_train variable.
      Similarly, information from subject_test and y_test is merged with the x_test variable.
      The appended columns were renamed subject_id and activity.
9.  SubFeatures variable captures the column numbers of column/variable names with -mean()
      or - std() sinc these are the only columns for the aggregation, and includes the subject_id and activity columns for the subgrouping.
10. AllData merges the rows from x_train and x_test, thus, combining the training
      and the test data.  The descriptive variable name for the activity is also
      added to the data frame.          
11. ActivitySet calculates the average of each measurable column per activity using
      the desc_activity column.
12. SubjectSet calculates the average of each measurable column per individual subject
      using the subject_id column.  Note that the Group.1 column needs to be transformed
      into a character type before SubjectSet is merged with ActivitySet.
13. TidySet combines the rows from SubjectSet and ActivitySet.  The final text output file,
      "Output_Mean_Grouped.txt", is based on this data frame.


Note that the Group.1 column of the SubjectSet and TidySet data frames has been transformed into
a character class to prevent NA values when merging rows and exporting to a text file,
respectively.

VARIABLES / COLUMNS OF THE OUTPUT TIDYSET DATA FRAME / TidySetColumns.txt FILE
1. These are the output columns of the resulting data frame 'TidySet'.
The information from the TidySet data frame is exported to "Output_Mean_Grouped.txt"
with " " delimiter.
2. Group.1 indicates which subset the mean value from each of the columns is associated to.
This could be from one of the activities performed by the subject individuals, or
a value from '1' to '30' (corresponding to the subject individual).     
3.  Columns/Variables listed below are the averages of the values of the -mean()
and -std() columns for each group

tBodyAcc.mean...X          
tBodyAcc.mean...Y          
tBodyAcc.mean...Z          
tBodyAcc.std...X           
tBodyAcc.std...Y           
tBodyAcc.std...Z           
tGravityAcc.mean...X       
tGravityAcc.mean...Y       
tGravityAcc.mean...Z       
tGravityAcc.std...X        
tGravityAcc.std...Y        
tGravityAcc.std...Z        
tBodyAccJerk.mean...X      
tBodyAccJerk.mean...Y      
tBodyAccJerk.mean...Z      
tBodyAccJerk.std...X       
tBodyAccJerk.std...Y       
tBodyAccJerk.std...Z       
tBodyGyro.mean...X         
tBodyGyro.mean...Y         
tBodyGyro.mean...Z         
tBodyGyro.std...X          
tBodyGyro.std...Y          
tBodyGyro.std...Z          
tBodyGyroJerk.mean...X     
tBodyGyroJerk.mean...Y     
tBodyGyroJerk.mean...Z     
tBodyGyroJerk.std...X      
tBodyGyroJerk.std...Y      
tBodyGyroJerk.std...Z      
tBodyAccMag.mean..         
tBodyAccMag.std..          
tGravityAccMag.mean..      
tGravityAccMag.std..       
tBodyAccJerkMag.mean..     
tBodyAccJerkMag.std..      
tBodyGyroMag.mean..        
tBodyGyroMag.std..         
tBodyGyroJerkMag.mean..    
tBodyGyroJerkMag.std..     
fBodyAcc.mean...X          
fBodyAcc.mean...Y          
fBodyAcc.mean...Z          
fBodyAcc.std...X           
fBodyAcc.std...Y           
fBodyAcc.std...Z           
fBodyAccJerk.mean...X      
fBodyAccJerk.mean...Y      
fBodyAccJerk.mean...Z      
fBodyAccJerk.std...X       
fBodyAccJerk.std...Y       
fBodyAccJerk.std...Z       
fBodyGyro.mean...X         
fBodyGyro.mean...Y         
fBodyGyro.mean...Z         
fBodyGyro.std...X          
fBodyGyro.std...Y          
fBodyGyro.std...Z          
fBodyAccMag.mean..         
fBodyAccMag.std..          
fBodyBodyAccJerkMag.mean..
fBodyBodyAccJerkMag.std..  
fBodyBodyGyroMag.mean..    
fBodyBodyGyroMag.std..     
fBodyBodyGyroJerkMag.mean..
fBodyBodyGyroJerkMag.std..

OTHER NOTES:
More detailed information on the run_analysis.R script is in the README.md file.
