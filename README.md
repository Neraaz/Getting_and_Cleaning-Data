        
              Activity
      ................................        
        
        V1                 V2
        1             WALKING
        2      WALKING_UPSTAIRS
        3      WALKING_DOWNSTAIRS
        4              SITTING
        5             STANDING
        6               LAYING

      *****See codebook for more information about different measurement.************                                         
       
       
                                           Instructions:


## At first, we check that the folder data already exists or not. If not then we download the file and put it on
## folder named data. Dataset.zip is our zipped folder. We use following command for this.
file.exists() to check whether the file exists or not?
dir.create() to create directory
download.file() to download the file.

## Now we unzip the file by using command "unzip". Again, we named unzip folder as data.
unzip()
## Inside data, there is another folder named "UCI HAR Dataset", Now we will proceed to get 
## data inside this folder.

path <- file.path() 


## Now we read different files containing Activity, Subject, Features of two different data
## sets "test" and "train" using read.table() function.

read.table()
## Now we merge the data tables by rows.
rbind()
## Now we set the data names into variables 
names()

## Now we merge Columns of the data tables which is formed by merging data tables by row earlier.
cbind()

## Now we extract only the measurement on the mean and standard deviation for each measurement.
grep()
selectedNames<-c(as.character(subdataFeaturesNames), "subject", "activity" )
Data<-subset(Data,select=selectedNames)

## Now we read descriptive activity names from "activity_labels.txt"
activityLabels <- read.table(file.path(path_rf, "activity_labels.txt"),header = FALSE)

## Now we label names of features using descriptive variable names.
names(Data)<-gsub("^t", "time", names(Data))
names(Data)<-gsub("^f", "frequency", names(Data))
names(Data)<-gsub("Acc", "Accelerometer", names(Data))
names(Data)<-gsub("Gyro", "Gyroscope", names(Data))
names(Data)<-gsub("Mag", "Magnitude", names(Data))
names(Data)<-gsub("BodyBody", "Body", names(Data))

# Now we use "plyr" package to get tidy data set.
library(plyr);
Data2<-aggregate(. ~subject + activity, Data, mean)
Data2<-Data2[order(Data2$subject,Data2$activity),]
write.table(Data2, file = "tidydata.txt",row.name=FALSE)

# writing codebook.
write(names(Data), file = "codebook.Rmd", ncolumns = 1)