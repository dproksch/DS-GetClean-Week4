# DS-GetClean-Week4
Coursera GetCleanData Week 4 Repo

# GettingAndCleaningData
GitHub repository for scripts used in the GettingAndCleaningData class project

This project contains the results of the Coursera class
Getting and Cleaning Data: https://class.coursera.org/getdata-013

## Requirements - as taken from the above referenced course page:
You should create one R script called run_analysis.R that does the following. 

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

## Project Contents:

- README.md - This file
- tidyds.txt - The file produced in step 5 of the assignment
- run_analysis.R - R script required to manipulate the data and produce the file in step 5
- CodeBook.md - Codebook explaining the data contained in the output file

## The process that was followed to create the required tidy dataset output

This project was completed using RStudio and began by creating a directory for this project and then setting the current working directory for RStudio to this working area.  The data was then downloaded from the site pointed to by the assignment notes: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip.  There was also a page that described how the original study was conducted: http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones.

After reading the description and downloading the data file, the file was then unzipped into the working directory.

#### The run_analysis.R script:

To produce the tidy dataset, the work is done in 3 steps:

1. Read in all the required data

    There were two datasets required for labels:
    - features.txt - for column labels - **
    - activity_labels.txt - for activity descriptions - this data was read in with two columns activityID and activity
    
    And 3 datasets for each train and test measurements each within their corresponding sub-directories:
    - X_train.txt - the actual measurement data for the train measurements
    - Y_train.txt - the activies being performed by the subject being measured - this data was read in with one column - activityID
    - subject_train.txt - the subjects who performed the activities being measured - this data was read in with one column - subject 
    - X_test.txt - the actual measurement data for the test measurements
    - Y_test.txt - the activies being performed by the subject being measured - this data was read in with one column - activityID
    - subject_test.txt - the subjects who performed the activities being measured - this data was read in with one column - subject 
   

2. Union the data sets needed for the output

    There were two processes involved here:
    + Bring each of the 3 datasets for testing and 3 datasets for training together as one dataset for test and one for train
    + Bring together the test and train datasets into a single dataset
    
    The 3 datasets were all the same number of observations so adding the additional columns to the measurement data with cbind was simple.  After this step - both requirement 1 and requirement 4 were met.
    
3. Prepare the output and write it to a file

    This was the most complex of the steps but was performed with a single dplyr statement followed by writing the tidy dataset.  The statements were chained so here are the steps performed:
    
    + Start with the dataset produced out of the previous step
    + Bring in the descriptive activity names for each activity by using merge - Meets step 3 requirement -
      This step needs to occur here because the merge column activityID will drop out in the next step...
    + Pull out only the mean and standard deviation columns using select - Meets step 2 requirement
    + Group the data for the final summarization - required for the next step to assist in meeting step 5 requirement
    + Summarize the data by averaging each measurement column (that is not in the group_by clause above) - assist in meeting step 5 requirement
    
    Finally the dataset is written out into the local working directory as tidyds.txt.
    
    The data was left in the "wide" form because it did not violate the principles of tidy data outlined in Hadley Wickham's paper: http://vita.had.co.nz/papers/tidy-data.pdf
    
    It follows the three principles:
      + Each variable forms a column.
      + Each observation forms a row.
      + Each type of observational unit forms a table.

    Rhe tidy dataset was validated by reading the dataset back in using:
    
    `validate_tidyds<-read.table("tidyds.txt", header=TRUE)`
    
    After comparing the tables, the original one was written to disk- using all.equal(tidyDS, validate_tidyds)  which showed the data was exactly the same in the two datasets
    

