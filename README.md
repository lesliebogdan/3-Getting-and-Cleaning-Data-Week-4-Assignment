# 3-Getting-and-Cleaning-Data-Week-4-Assignment
Coursera Course: Getting and Cleaning Data

*Purpuse of this repo is to answer the Week 4 assignment from the 'Getting and Cleaning Data' subject

*Below will now explain the steps being carried out in 'run_analysis.R' to get to the final 'tidy' data output*

#LESLIE

##LESLIE

#Step 1
download data set (still in its zipped form) and have it access to it in your working directory
Call the file 'Project.zip'

#Step 2
Unzip the file and store in a vector the names of all the files you downloaded, call it 'list_of_files'

The vector will return the below, I like to reference index positions when I need to load files (saves from having to type out the file name)

#return below is for 'list_of_files'

#[1] "./UCI HAR Dataset/activity_labels.txt"                         
#[2] "./UCI HAR Dataset/features.txt"                                
#[3] "./UCI HAR Dataset/features_info.txt"                           
#[4] "./UCI HAR Dataset/README.txt"                                  
#[5] "./UCI HAR Dataset/test/Inertial Signals/body_acc_x_test.txt"   
#[6] "./UCI HAR Dataset/test/Inertial Signals/body_acc_y_test.txt"   
#[7] "./UCI HAR Dataset/test/Inertial Signals/body_acc_z_test.txt"   
#[8] "./UCI HAR Dataset/test/Inertial Signals/body_gyro_x_test.txt"  
#[9] "./UCI HAR Dataset/test/Inertial Signals/body_gyro_y_test.txt"  
#[10] "./UCI HAR Dataset/test/Inertial Signals/body_gyro_z_test.txt"  
#[11] "./UCI HAR Dataset/test/Inertial Signals/total_acc_x_test.txt"  
#[12] "./UCI HAR Dataset/test/Inertial Signals/total_acc_y_test.txt"  
#[13] "./UCI HAR Dataset/test/Inertial Signals/total_acc_z_test.txt"  
#[14] "./UCI HAR Dataset/test/subject_test.txt"                       
#[15] "./UCI HAR Dataset/test/X_test.txt"                             
#[16] "./UCI HAR Dataset/test/y_test.txt"                             
#[17] "./UCI HAR Dataset/train/Inertial Signals/body_acc_x_train.txt" 
#[18] "./UCI HAR Dataset/train/Inertial Signals/body_acc_y_train.txt" 
#[19] "./UCI HAR Dataset/train/Inertial Signals/body_acc_z_train.txt" 
#[20] "./UCI HAR Dataset/train/Inertial Signals/body_gyro_x_train.txt"
#[21] "./UCI HAR Dataset/train/Inertial Signals/body_gyro_y_train.txt"
#[22] "./UCI HAR Dataset/train/Inertial Signals/body_gyro_z_train.txt"
#[23] "./UCI HAR Dataset/train/Inertial Signals/total_acc_x_train.txt"
#[24] "./UCI HAR Dataset/train/Inertial Signals/total_acc_y_train.txt"
#[25] "./UCI HAR Dataset/train/Inertial Signals/total_acc_z_train.txt"
#[26] "./UCI HAR Dataset/train/subject_train.txt"                     
#[27] "./UCI HAR Dataset/train/X_train.txt"                           
#[28] "./UCI HAR Dataset/train/y_train.txt" 

#Step 3
Create a data.table which has the feature names in it (these will be used to generate the variable headings of measurements a little later 

#Step 4 a
Load in the test dataset into a data table
Set the names of the measuresments (columns) in the data table to the 'feature' names from Step 3

#Step 4 b
Load the 'Activity Description' into a seperate table (pairs activity numbers with their descriptions), you will pair these with the measurement data set shortly



#Step 5
Do what you did in steps 4a and 4b, but this time do it for the 'train' dataset

You Will now have two datasets with identical column names (different number of records though)

#Step 6
Combine the two datasets together (test and train)



#Step 7

Read in the activity labels you want to pair with the activity numbers
join in the activity description labels on their respective number representations 

#Step 8
Remove measurements (columns) which do not relate to either mean() or std() measurements.

#Step 9
Clean up the column headers (variable names)
Give human readable labels (short hand t and f prefix changed to thier full names), also give full names to measure summaries (mean value and standard deviation)

#Step 10
Create a new data table (copy the results you have generated in the above steps)

#Step 11
Create a data table with the subject codes for both the 'train' and 'test' datasets
combine the two data tables (make sure the order is the same as in step 6 (very important)
With new data table, 
Join the subject data table

#Step 12
use the 'aggregate' function over the data table to return the mean for each activity / subject combination 

#END
########################








