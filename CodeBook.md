#getting-and-cleaning-data-assignment
#By Sen 
#Introduction
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

#Preparation and data downloading
if (!file.exists("./data")){
        dir.create("./data")
}
fileUrl="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "./data/rundata.zip",method = "curl" )
unzip("./data/rundata.zip", exdir = "./data/")
setwd("./data/")

features and data reading

features <- read.table("./UCI HAR Dataset/features.txt", header = F,stringsAsFactors=FALSE)
test.data<-read.table("./UCI HAR Dataset/test/X_test.txt", header = F)
test.labels<-read.table("./UCI HAR Dataset/test/y_test.txt",col.names="label")
test.subjects <- read.table("./UCI HAR Dataset/test/subject_test.txt", col.names="subject")
training.data<-read.table("./UCI HAR Dataset/train/X_train.txt",header = F)
training.labels <- read.table("./UCI HAR Dataset/train/y_train.txt", col.names="label")
training.subjects <- read.table("./UCI HAR Dataset/train/subject_train.txt", col.names="subject")
#Step1. Merges the training and the test sets to create one data set.

adding variable names
colnames(test.data)<- features[,2]
colnames(training.data)<- features[,2]

assembling and merging test and training data set
merge.data <- rbind(cbind(test.subjects, test.labels, test.data),
              cbind(training.subjects, training.labels, training.data))

#Step2. Extracts only the measurements on the mean and standard deviation for each measurement.

grep features of mean and standard deviation, be careful only grep mean or std followed by ()
features.mean.std <- features[grep("mean\\(\\)|std\\(\\)", features$V2), ]

Add 2 columns because first two columns from merge.data are subjects and labels 
data.mean.std<-merge.data[,c(1,2,features.mean.std$V1+2)]

#Step3. Uses descriptive activity names to name the activities in the data set
labels<-read.table("./UCI HAR Dataset/activity_labels.txt", header = F)
data.mean.std$label<-labels[data.mean.std$label, 2]

#Step4. Appropriately labels the data set with descriptive variable names.

create new column name vector
tidy.names<-c("subject","label",features.mean.std$V2)
#remove non-alphabetic characters
tidy.names<-gsub("[^[:alpha:]]", "", tidy.names)
#replace abbreviative letter in the beginning of the variables 
tidy.names <- gsub("^f", "Frequency", tidy.names)
tidy.names <- gsub("^t", "Time", tidy.names)
colnames(data.mean.std)<-tidy.names

#Step5.Creates a second, independent tidy data set with 

the average of each variable for each activity and each subject.
library(plyr)
newdata<- ddply(data.mean.std, c("subject","label"),numcolwise(mean))
write.table(newdata, file = "newdata.txt")

Subject column is numbered sequentially from 1 to 30. Activity column has 6 types as listed below assigned with a numeric activity id.

   1. WALKING
   2. WALKING_UPSTAIRS
   3. WALKING_DOWNSTAIRS
   4. SITTING
   5. STANDING
   6. LAYING

Other variables are detailed in features_info.txt along with the dataset.
