# Getting_and_cleaning_data_assessment
Peer grade assessment of the course Getting and Cleanning Data offered by Coursera and the Jhon Hopkins University

######BEFORE RUNNING YOUR SCRIPT, MAKE SURE YOU DOWNLOADED AND EXTRACTED THE FOLDER "UCI HAR Dataset" IN YOUR CURRENT R DIRECTORY. TO KNOW WHICH IS ITS NAME YOU  CAN USE THE FUNCTION "GETWD()".

DESCRPITION OF THE SCRIPT
The structure of the script is divided in parts and subparts. Parts are identified by a single number sign, and subparts are defined as a part of a part or another subpart. They can be identified by 2 number signs for the first level subparts (part of a part). Plus number signs can be added to identify subparts of subparts in the way they could be appear.  
The script is divided into the following parts:
1. FILES LOCATION: Before running the script, you should download and extract the folder "UCI HAR Dataset" in your current R directory. to know which is its name you  can use the function "getwd()". 
this part of the script creates a list of objects with the name and location of the script's files that will be used in further steps.
2.READ DATA OF INTEREST: Read the data required by the analysis and store it in R objects. The subjects are charged in a data frame that identifies them with the analysis that was performed to them (test group or train group). The subparts in these segment are the following:
Headers: set the labels of the variables of study and the names of the activities.
Subjects evaluated: are identified by numbers from 1 to 30
Activity codes: activities are coded in the database with numbers from 1 to 6. the labels of those activities can be foun in the variables description
Statistics calculated from the measuraments: Load the measurements calculated from the observations. To further information read the "about the data" section.
3.CONSTRUCTION OF THE MAIN DATA FRAME: Is the procces of debuging the data frame and set a single data frame with the variables of interest. It is divided into the following subparts:
Identify the measurements of interest: parse the headers from the data frame and identifies which ones contain mean and standard deviation measurements
Make study data frames: Construction of two data frames, one for each set of study (train and test) with the variables of interest.
Join data frames: binding of the two data frames created in the previous step into a single one
Labeling activities: Merge the data frame with the activity indicator to label using the code of activity codes.
4. AVERAGE OF MEASUREMENTS FOR ACTIVITY AND SUBJECT TABLE: From the previous data set creates a second, independent tidy data set with the average of each variable for each activity and each subject.

