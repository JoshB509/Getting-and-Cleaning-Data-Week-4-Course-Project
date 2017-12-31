This file first gives a description of the steps taken in order to complete the 5 tasks as part of Getting-and-Cleaning-Data-Week-4-Course-Project.
You will find the output "Tidydata' file as well as the R Script (saved in the README file) in this repo

Task 1:
Step 1 - Read the 'Features' 'Subject' and 'Activity' test & training data from the provided datasets using the read.table command
Step 2 - Using row bind, i merged the training and test datasets by coresponding table, i.e. Test Activity with Training Activity
Step 3 - Give the Activity and Subject vectors appropiate colnames. Read the 'features' data from the provided dataset and apply them to the features vector
Step 4 - Combine the activity, subject and features datasets together using cbind


Task 2:
Step 1 - Create a vector that reads the data features that contain the string 'mean' or 'std' and ignores the '//'
Step 2 - Format that vector into characters and add 'Activity' and 'Subject' 
Step 3 - Subset my data to only the values that match the values of the created vector. As the created vector is a list of all the mean and std features, this will leave me with dataset of only hte mean and std valeus of each measurement

Task 3:
Step 1 - Read the activity Label data from the provided datasets
Step 2 - Convert the 'activity' colum in my dataset to a factor
Step 3 - Match the now factor values of the 'activity' colum in my dataset to the labels in the data labels table

Task 4:
Step 1 - Using the package 'plyr' and the gsub command, I replace unclean data elements, such as 'Mag' with more descriptive names

Task 5:
Step 1 - Create a new vector that aggregates the values from the dataset. This is achieved with the 'aggregate' command that I pass the dataset, the required collumns and ask for the mean average
