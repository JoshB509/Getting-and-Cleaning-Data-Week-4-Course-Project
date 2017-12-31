# Getting-and-Cleaning-Data-Week-4-Course-Project


## Task 1 Merge the training and the test sets to create one data set. --------------------------------------------------------------------------------------------------

## Load plyr to be used later
library(plyr)

## Read training datasets
dataActivityTrain <- read.table("./train/y_train.txt", header=FALSE)
dataFeaturesTrain <- read.table("./train/X_train.txt", header=FALSE)
dataSubjectTrain <- read.table("./train/subject_train.txt", header=FALSE)

## Read the Test datasets
dataActivityTest <- read.table("./test/y_test.txt", header=FALSE)
dataFeaturesTest <- read.table("./test/X_test.txt", header=FALSE)
dataSubjectTest <- read.table("./test/subject_test.txt", header=FALSE)

## Merge together by row the corresponding training and datasets
dataSubject <- rbind(dataSubjectTrain, dataSubjectTest)
dataActivity<- rbind(dataActivityTrain, dataActivityTest)
dataFeatures<- rbind(dataFeaturesTrain, dataFeaturesTest)

## Set names and in Subject and Activity datasets and read form features file an apply the names from teh file to the features vector
names(dataSubject) <-c("subject")
names(dataActivity) <-c("activity")
dataFeaturesNames <- read.table("./features.txt",header=FALSE)
names(dataFeatures)<- dataFeaturesNames$V2

##Combine the subject and activity datasets, then bind them with the features
dataCombine <- cbind(dataSubject, dataActivity)
Data <- cbind(dataFeatures, dataCombine)

## Task 2 Extract only the measurements on the mean and standard deviation for each measurement.--------------------------------------------------------------------------------------------------

## create a vector with the values that contain the strings mean or std, ignoring the slashes
subdataFeaturesNames <- dataFeaturesNames$V2[grep("mean\\(\\)|std\\(\\)", dataFeaturesNames$V2)]

## create a vector with the means and STD as characters with "subject" and "activity" added
selectedNames <- c(as.character(subdataFeaturesNames), "subject", "activity" )

## subset the data that matches the values in 'selectNames' which will be jus the mean and STD for each measurement
Data <- subset(Data, select=selectedNames)

## Task 3 Uses descriptive activity names to name the activities in the data set.--------------------------------------------------------------------------------------------------

## Read activity label table
activityLabel <- read.table("./activity_labels.txt",header=FALSE)

## Vactorise the 'activity' comlum by using the labels in the activity label table
Data$activity <- factor(Data$activity);
Data$activity <- factor(Data$activity, labels = as.character(activityLabel$V2))

## Task 4 Appropriately labels the data set with descriptive variable names..--------------------------------------------------------------------------------------------------

##Using gsub, replace unclean name elements with more descriptive ones

names(Data) <- gsub("Acc", "Accelerometer", names(Data))
names(Data) <- gsub("Gyro", "Gyroscope", names(Data))
names(Data) <- gsub("Mag", "Magnitude", names(Data))
names(Data) <- gsub("BodyBody", "Body", names(Data))
names(Data) <- gsub("^t", "Time", names(Data))
names(Data) <- gsub("^f", "Frequency", names(Data))

## Task 5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject. --------------------------------------------------------------------------------------------------

Final_Data <- aggregate(. ~subject + activity, Data, mean)
Final_Data <- Final_Data[order(Final_Data$subject,Final_Data$activity),]

## Write the dataset to a file called TidyData
write.table(Final_Data, file = "tidydata.txt",row.name=FALSE)
