# 3-Getting-and-Cleaning-Data-Week-4-Assignment
Coursera Course: Getting and Cleaning Data

*Purpuse of this repo is to answer the Week 4 assignment from the 'Getting and Cleaning Data' subject

*Below will now explain the steps being carried out in 'run_analysis.R' to get to the final 'tidy' data output*

#LESLIE#
##LESLIE##

#########################
#create a temp file location and download the zip file to it.
#temp <- tempfile()
#download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip",temp)
#########################

#Load from wd()
#assume you have downloaded the file and it is called 'Project.zip'
temp<-"Project.zip"

#########################
#stores list of all files that were downloaded
list_of_files<-unzip(temp)
#########################

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

#########################
# publish to table the 'features.txt'
df_features<-read.table(list_of_files[2])
#create a character vector that has the names of the columns (will use the later)
# Need to convert the result (factor) to a character vector, this will then be used for column titles
df_features_name<-as.character(df_features[,2])
#########################

#########################
#load the dataset body for the 'test' group
df_test<-read.table(list_of_files[15])
#load the measurements to pair with the actions
df_names_test<-read.table(list_of_files[16])
names(df_names_test)[1]<-"Action"
names(df_test)[1:561]<-df_features_name
df_test_merge<-cbind(df_names_test,df_test)
#########################

#########################
#do what you did for the 'test' dataset for the 'train' set
df_train<-read.table(list_of_files[27])
df_names_train<-read.table(list_of_files[28])
names(df_names_train)[1]<-"Action"
names(df_train)[1:561]<-df_features_name
df_train_merge<-cbind(df_names_train,df_train)
#########################

#########################
#combine the 'test' and 'train' datasets 
#Part 1 of the task
df_merge<-rbind(df_train_merge,df_test_merge)
#########################

#########################
#Part 2 - only retain the measurements that relate to 
# 1:mean(): Mean value
# 2:std(): Standard deviation
# want to only keep columns with either 'mean()' or 'std()' strings anywhere in the column name 
#use '\\b' boundries to return exact matches
df_merge_keep<-df_merge[ , grep("\\bmean()\\b|\\bstd()\\b|\\bAction\\b", colnames(df_merge))]
#########################


#########################
#PART 3: add description to activities ('Action Description')
#Need to join in 'descriptive' activity names to the dataset ('Action' column name in my dataset with their descriptions)
#read in 'activity_labels.txt
df_activity_labels<-read.table(list_of_files[1])
#give the table useful names 'Action' used in the 'df_merge' table we will be joining to shortly.
names(df_activity_labels)<-c("Action","Action Description")

#merge the actions to the measurements table
df_merge_keep_activity_labels<-merge(df_merge_keep,df_activity_labels,by="Action",all.x=TRUE)
#########################


#########################
#Part 4: Clean up the variable names
#prefix 'f' denotes 'frequency' - add this to the variable name
names(df_merge_keep_activity_labels)<- sub("^f","frequency-",names(df_merge_keep_activity_labels))
#prefix 't' denotes 'time' - add this to the variable name
names(df_merge_keep_activity_labels)<-sub("^t","time-",names(df_merge_keep_activity_labels))

#replace 'mean' with 'mean value'
names(df_merge_keep_activity_labels)<-sub("\\bmean()\\b","mean value",names(df_merge_keep_activity_labels))
#replace 'std' with 'standard deviation'
names(df_merge_keep_activity_labels)<-sub("\\bstd()\\b","standard deviation",names(df_merge_keep_activity_labels))
#########################

#########################
#Part 5: create a new data set where you have the 'subject' and 'activity' and have the average for each measurement returned
#copy the dataframe from part 4 to a new variable ('df_avg')
df_avg<-df_merge_keep_activity_labels

#First need to get the 'subject' codes into the data frame
df_subject_train<-read.table(list_of_files[26])
df_subject_test<-read.table(list_of_files[14])



#Need to combine the 'df_subject_train' and the 'df_subject_test' in the same order as the previous data was combined
#REFERENCE for order =>> df_merge<-rbind(df_train_merge,df_test_merge)
df_subject<-rbind(df_subject_train,df_subject_test)
names(df_subject)[1]<-"Subject"

#Now need to combine the subjects into the 'df_avg' data frame
df_avg_subject<-cbind(df_avg,df_subject)
#make the 'Action Description' a character so you can use aggregate functions on it shortly (cant aggregate on factors)
df_avg_subject$`Action Description`<- as.character(df_avg_subject$`Action Description`)

df_final<-aggregate(df_avg_subject,by=list(df_avg_subject$`Action Description`,df_avg_subject$Subject),FUN=mean)



names(df_final)[1]<-"Activity"
names(df_final)[2]<-"Subject"

#########################

#########################
#disconnect from the download
#unlink(temp)
#########################
