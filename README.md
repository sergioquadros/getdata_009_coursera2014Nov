---
title: "README"
author: "Sérgio Henrique Silveira de Quadros"
date: "2014-11-22"
output: html_document
---
##  README

_Ubuntu 14.04 LTS =  i686-pc-linux-gnu (32-bit)_
_R version 3.1.2 (2014-10-31) -- "Pumpkin Helmet"_
_RStudio version 0.98.1074_

Our script isn't **OS** agnostic, this means that it was maked in an linux environment and for another one, maybe it's necessary some adjustments. 

Access [run_analisys.R](<https://github.com/sergioquadros/getdata_009_coursera2014Nov/blob/master/run_analisys.R>) script here or in a copy below.

This script produces three files in plain text format:

+  "**first_tidy.txt**": the tidy file by Hadley Wickham' principles;

+  "**tidy2.txt**": a summarizing file by mean and sd for each column;
 
+  "**tidy3.txt**": a summarizing file by mean for each column after group tidy data by __"subject"__ and __"activity"__.
  

If you want to run this script, command this in your R console (like RStudio) at your working directory:
```source(file="./run_analysis.R", local = TRUE, echo = TRUE)```  

The run_analisys.R script:

```
# shsq Coursera: Getting and Cleaning Data/2014-11
# Ubuntu 14.04 LTS =  i686-pc-linux-gnu (32-bit)
# R version 3.1.2 (2014-10-31) -- "Pumpkin Helmet"
# RStudio version 0.98.1074

# Warning: this script isn't OS agnostic because i shouldn't use parameters.
# If you want to run this script, command this in your R console
# (like RStudio) at your working directory:
#       source(file="./run_analysis.R", local = TRUE, echo = TRUE)

# FIVE GOALS:
# 1. Merges the training and the test sets to create one data set.
# 2. Extracts only the measurements on the mean and standard deviation for each 
#    measurement. 
# 3. Uses descriptive activity names to name the activities in the data set
# 4. Appropriately labels the data set with descriptive variable names. 
# 5. From the data set in step 4, creates a second, independent tidy data set  
#    with the average of each variable for each activity and each subject.

# Calling necessary libraries.
library(data.table)
library(httr)
library(dplyr)
library(tidyr)

# Establishing working directories.
workdir <- c("","","","")
workdir[1] <- getwd()
workdir[2:4]<- c("./UCI HAR Dataset","./UCI HAR Dataset/test",
                 "./UCI HAR Dataset/train")

# Download and unzip
download.file("http://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
              ,destfile="zipado.zip")
unzip(zipfile="zipado.zip", files = NULL, list = FALSE, overwrite = TRUE,
      junkpaths = FALSE, exdir = workdir[1], unzip = "internal",
      setTimes = FALSE)

# Relative path of directories and files.
all<-list.files(workdir[2:4], full.names=TRUE)
# all
# [1] "./UCI HAR Dataset/activity_labels.txt"           # 1st pre-process
# [2] "./UCI HAR Dataset/features_info.txt"      
# [3] "./UCI HAR Dataset/features.txt"                  # 2nd pre-process
# [4] "./UCI HAR Dataset/README.txt"             
# [5] "./UCI HAR Dataset/test"                   
# [6] "./UCI HAR Dataset/test/Inertial Signals"  
# [7] "./UCI HAR Dataset/test/subject_test.txt"         # 3rd pre-process
# [8] "./UCI HAR Dataset/test/X_test.txt"               # 5th pre-process
# [9] "./UCI HAR Dataset/test/y_test.txt"               # 4th pre-process        
# [10] "./UCI HAR Dataset/train"                  
# [11] "./UCI HAR Dataset/train/Inertial Signals" 
# [12] "./UCI HAR Dataset/train/subject_train.txt"      # 6th pre-process
# [13] "./UCI HAR Dataset/train/X_train.txt"            # 8th pre-process
# [14] "./UCI HAR Dataset/train/y_train.txt"            # 7th pre-process

#activity_labels<- as.character(read.table(all[1])[,2]) # 1st pre-process

features <- as.character(read.table(all[3])[,2])        # 2nd pre-process
# GOAL 4: appropriate features' labels without repetitions
# and forbidden characters. There are 561 names with 84 ones repeated.
features <- make.names(features, unique = TRUE) 

subject_test <- tbl_df(read.table(all[7]))              # 3rd pre-process
# GOAL 4: appropriate label
colnames(subject_test) <- "subject"
subject_unique_test <- subject_test %>% unique %>% mutate(status_quo="test")
activity_test0 <- read.table(all[9])                    # 4th pre-process
activity_test <- activity_test0 
# GOAL 3: uses descriptive activity names to name the activities in the data set
activity_test[activity_test0==1] <- "WALKING"            
activity_test[activity_test0==2] <- "WALKING_UPSTAIRS"
activity_test[activity_test0==3] <- "WALKING_DOWNSTAIRS"
activity_test[activity_test0==4] <- "SITTING"
activity_test[activity_test0==5] <- "STANDING"
activity_test[activity_test0==6] <- "LAYING"    
activity_test <- tbl_df(activity_test)
# GOAL 4: appropriate label
colnames(activity_test) <- "activity"

x_test <- tbl_df(read.table(all[8]))                    # 5th pre-process
colnames(x_test) <- features

test <- cbind(cbind(subject_test,activity_test),x_test) # First set to merge

#################################################

# SIMILAR WAY ABOUT GOALS WITH TRAIN SET

subject_train <- tbl_df(read.table(all[12]))            # 6th pre-process
colnames(subject_train) <- "subject"
subject_unique_train <- subject_train %>% unique %>% mutate(status_quo="train")
# GOAL 3: uses descriptive activity names to name the activities in the data set
activity_train0 <- read.table(all[14])                  # 7th pre-process
activity_train <- activity_train0
activity_train[activity_train0==1] <- "WALKING"            
activity_train[activity_train0==2] <- "WALKING_UPSTAIRS"
activity_train[activity_train0==3] <- "WALKING_DOWNSTAIRS"
activity_train[activity_train0==4] <- "SITTING"
activity_train[activity_train0==5] <- "STANDING"
activity_train[activity_train0==6] <- "LAYING"    
activity_train <- tbl_df(activity_train)
# GOAL 4: appropriate label
colnames(activity_train) <- "activity"

x_train <- tbl_df(read.table(all[13]))                  # 8th pre-process
colnames(x_train) <- features

train <-cbind(cbind(subject_train,activity_train),
              x_train)                                  # Second set to merge

##################################################

# GOAL 1: it merges test and train sets in a tidy file="first_tidy.txt".
first_tidy <- rbind_list(test,train)
subject0 <- rbind_list(subject_unique_train,subject_unique_test)
subject <- subject0 %>% group_by(status_quo) %>% arrange(subject)
# For my use, this write "all_subject.txt" that links subjects to 
# test or train sets.
write.table(subject,file="all_subject.txt",row.name=FALSE)
write.table(first_tidy,file="first_tidy.txt",row.name=FALSE)          

##################################################

x <- first_tidy %>%
        select(3:563) %>% 
                summarise_each(funs(mean,sd))%>%
                        as.numeric
tidy2 <- matrix(rep(0.0,times=1122), nrow = 2, ncol = 561)
colnames(tidy2) <- features
rownames(tidy2)<-c("mean","sd")
tidy2[1,1:561] <-x[1:561]
tidy2[2,1:561] <- x[562:1122]

# GOAL 2: extracts only the measurements on the mean and standard deviation  
# for each measurement that ENDS WITH a summary file=tidy2.txt
write.table(tidy2,file="tidy2.txt", row.name=TRUE)

##################################################

# GOAL 5: from step 4, this creates a second summary of tidy data set with the
# average of each variable for each activity and each subject in "tidy3.txt" file.
xx <- first_tidy %>%
                select(1:563) %>%
                        group_by(subject,activity) %>%
                                summarise_each(funs(mean))
write.table(xx,file="tidy3.txt",row.name=FALSE)
```
