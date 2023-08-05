# Week-5-Piping
Piping, clearly expressing a sequence of multiple operations.
#Lines 5 through 20 are examples of various file types 
#and the code to read and write them. 
#Your tasks begin at line 22.

#Getting and saving your dataset is typically a two step process
#Read and write a delimited text file.
#datasetname <- read.table(‘file.txt’)
#write.table(datasetname, ‘file.txt’)

#Read and write a comma separated value file. This is a special case of read.table/ write.table.	
#datasetname <- read.csv(‘file.csv’)
#write.csv(datasetname, ‘file.csv’)

#Read and write an R data file, a file type special for R.	
#load(‘file.RData’)
#save(datasetname, file = ‘file.Rdata’)

#Read and write an R data file from GitHub.
#You need to select 'raw data' on the GitHub page 
#and then copy the URL and put in your code, as below

#TASK: run the code below to get and save the dataset
download.file(url = "https://raw.githubusercontent.com/fivethirtyeight/data/master/airline-safety/airline-safety.csv", destfile = "airline_safety.csv")


#Then you need to name your dataset
airline_safety<- read.csv("airline_safety.csv")

airline_safety #airline_safety 56 obj. and 8 variables: 
#airline, avail_seat_km_per_week, incidents_85_99, fatal_accidents_85_99,fatalities_85_99,
#incidents_00_14, fatal_accidents_00_14, fatalities_00_14

#TASK: take a look at the airline safety data. 

#dataset presents represents 56 airline companies and how many incidents they had 
#in the period 1985 to 1999, and the period 2000 to 2014. Also, how many fatal accidents 
#there were out of the total number of incidents, and how many victims there were in those 
#accidents. All this in relation to the total number of kilometers traveled multiplied by 
#the number of seats.

#TASK: Install and call the dplyr package. 
install.packages("dplyr")
library("dplyr")

#Let's make a random sample of our data and save it
mysample<-sample_n(airline_safety, size=15, replace = FALSE, weight = NULL, .env = NULL)
#mysample run and saved

#TASK: Save the new sample as a csv file

#datasetname <- read.csv(‘file.csv’)
write.csv(mysample, 'mysampleDarko.csv')
write.csv(mysample, 'C:\\Users\\Darko\\OneDrive\\Desktop\\R script\\mysampleDarko.csv')
#saving CSV file to specific location on my desktop

#Now let's have some fun with *piping*

#we will use our mysample dataset
#The pipe, %>%, comes from the magrittr package. 
#Packages in the tidyverse (like dplyr) load %>% for you automatically, 
#so you don’t usually load magrittr explicitly.


#Example: Let's try some piping with our mysample data. Note how the dataset name is not repeated in each function
piping<-mysample %>% 
  mutate (seats = avail_seat_km_per_week) %>%
  subset(incidents_85_99 < 24) %>%
  dim()%>%
  print()

#TASK: revise this code chunk using piping
mysample2<-mysample
arrange(mysample2, airline)
mysample2<-filter(mysample2, incidents_85_99>10)
mysample2<-rename(mysample2, seats = avail_seat_km_per_week)
mysample3<-select(mysample2, incidents_00_14, incidents_85_99)
mysample4<-summary(mysample3)
print(mysample4)

#result checked, same as mysample4
piping<-mysample %>% 
  arrange(airline) %>%
  filter(incidents_85_99>10) %>%
  rename(seats = avail_seat_km_per_week) %>%
  select(incidents_00_14, incidents_85_99) %>%
  summary() %>%
  print()
