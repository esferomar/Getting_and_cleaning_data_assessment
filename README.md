# Getting_and_cleaning_data_assessment
Peer grade assessment of the course Getting and Cleanning Data offered by Coursera and the Jhon Hopkins University

######BEFORE RUNNING YOUR SCRIPT, MAKE SURE YOU DOWNLOADED AND EXTRACTED THE FOLDER "UCI HAR Dataset" IN YOUR CURRENT R DIRECTORY. TO KNOW WHICH IS ITS NAME YOU  CAN USE THE FUNCTION "GETWD()".

#FILES LOCATION
activity_labels<-paste(getwd(),"/UCI HAR Dataset/activity_labels.txt",sep="")
features<-paste(getwd(),"/UCI HAR Dataset/features.txt",sep="")
##test
subject_test<-paste(getwd(),"/UCI HAR Dataset/test/subject_test.txt",sep="")
X_test<-paste(getwd(),"/UCI HAR Dataset/test/X_test.txt",sep="")
y_test<-paste(getwd(),"/UCI HAR Dataset/test/y_test.txt",sep="")
##Test inertial signals
body_acc_x_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/body_acc_x_test.txt",sep="")
body_acc_y_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/boddy_acc_y_test.txt",sep="")
body_acc_z_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/body_acc_z_test.txt",sep="")
body_gyro_x_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/body_gyro_x_test.txt",sep="")
body_gyro_y_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/body_gyro_y_test.txt",sep="")
body_gyro_z_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/body_gyro_x_test.txt",sep="")
total_acc_x_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/total_acc_x_test.txt",sep="")
total_acc_y_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/total_acc_y_test.txt",sep="")
total_acc_z_test<-paste(getwd(),"/UCI HAR Dataset/test/Inertial Signals/total_acc_z_test.txt",sep="")
##train
subject_train<-paste(getwd(),"/UCI HAR Dataset/train/subject_train.txt",sep="")
X_train<-paste(getwd(),"/UCI HAR Dataset/train/X_train.txt",sep="")
y_train<-paste(getwd(),"/UCI HAR Dataset/train/y_train.txt",sep="")
##train Inertial Signals
body_acc_x_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/body_acc_x_train.txt",sep="")
body_acc_y_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/boddy_acc_y_train.txt",sep="")
body_acc_z_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/body_acc_z_train.txt",sep="")
body_gyro_x_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/body_gyro_x_train.txt",sep="")
body_gyro_y_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/body_gyro_y_train.txt",sep="")
body_gyro_z_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/body_gyro_x_train.txt",sep="")
total_acc_x_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/total_acc_x_train.txt",sep="")
total_acc_y_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/total_acc_y_train.txt",sep="")
total_acc_z_train<-paste(getwd(),"/UCI HAR Dataset/train/Inertial Signals/total_acc_z_train.txt",sep="")

#READ DATA OF INTEREST
##HEADERS
featuresbd<-read.csv(features,sep="",header=F)
featuresbd$V2<-sub("tBody","TimeBody",featuresbd$V2)
featuresbd$V2<-sub("tGravity","TimeGravity",featuresbd$V2)
featuresbd$V2<-sub("Acc","Accelerometer",featuresbd$V2)
featuresbd$V2<-sub("Mag","Magnitude",featuresbd$V2)
featuresbd$V2<-sub("Gyro","Gyroscope",featuresbd$V2)


activityLabels<-read.csv(activity_labels,sep="",header=F)
colnames(activityLabels)<-c("code_activity","activity")
##SUBJECT EVALUATED.   
sbjctTrain<-data.frame(read.csv(subject_train,sep=""),"train")
colnames(sbjctTrain)<-c("subject","set")
sbjctTest<-data.frame(read.csv(subject_test,sep=""),"test")
colnames(sbjctTest)<-c("subject","set")
##ACTIVITY CODES
ytrain<-read.csv(y_train,sep="")
colnames(ytrain)<-"code_activity"
ytest<-read.csv(y_test,sep="")
colnames(ytest)<-"code_activity"
##STATISTICS CALCULATED FROM THE MEASUREMENTS
xtrain<-read.csv(X_train,sep="")
colnames(xtrain)<-featuresbd$V2
xtest<-read.csv(X_test,sep="")
colnames(xtest)<-featuresbd$V2

#CONSTRUCTION OF THE MAIN DATA FRAME
##IDENTIFY THE MEASUREMENTS OF INTEREST (MEANS AND STANDARD DEVIATION)
means<-grep("mean()",featuresbd$V2)
stds<-grep("std()",featuresbd$V2)
##MAKE STUDY DATA FRAMES
bdtrain<-data.frame(sbjctTrain,ytrain,xtrain[means],xtrain[stds])
bdtest<-data.frame(sbjctTest,ytest,xtest[means],xtest[stds])
##JOIN DATA FRAMES
bdbind<-rbind(bdtrain,bdtest)
##LABELING ACTIVITIES 
bdbind<-merge(y=bdbind,x=activityLabels,by.x="code_activity",by.y="code_activity")
nrow(bdbind)
library(dplyr)
bdbind<-tbl_df(bdbind)
bdbind<-arrange(bdbind,subject,activity)

#AVERAGE OF MEASUREMENTS FOR ACTIVITY AND SUBJECT TABLE
bdcons<-bdbind%>%group_by(subject,activity)%>%
  summarise_at(c(3:81),mean)
