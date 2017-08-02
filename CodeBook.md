getting-and-cleaning-data-assignment
By Sen 
Introduction
The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.

One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained:

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Here are the data for the project:

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

You should create one R script called run_analysis.R that does the following 5 steps:

Part 1
Merges the training and the test sets to create one data set.
##preparation
#data downloading
if (!file.exists("./data")){
        dir.create("./data")
}
fileUrl="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl, destfile = "./data/rundata.zip",method = "curl" )
unzip("./data/rundata.zip", exdir = "./data/")
setwd("./data/")
#features and data reading
features <- read.table("./UCI HAR Dataset/features.txt", header = F,stringsAsFactors=FALSE)
#check structure
str(features)
test.data<-read.table("./UCI HAR Dataset/test/X_test.txt", header = F)
test.labels<-read.table("./UCI HAR Dataset/test/y_test.txt",col.names="label")
test.subjects <- read.table("./UCI HAR Dataset/test/subject_test.txt", col.names="subject")
training.data<-read.table("./UCI HAR Dataset/train/X_train.txt",header = F)
training.labels <- read.table("./UCI HAR Dataset/train/y_train.txt", col.names="label")
training.subjects <- read.table("./UCI HAR Dataset/train/subject_train.txt", col.names="subject")



