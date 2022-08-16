
## Case Study Google Data Analytics Certification: Bellabeat Fitness

### Scenario

Bellabeat is a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Their cofounder, and Chief Creative Officer, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. 
    
### Questions for Analysis

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers? 
3. How could these trends help influence Bellabeat marketing strategy?

### Business Task 

Analyze how consumers currently use smart fitness devices in order to determine marketing strategies for Bellabeat customers.

### Data 

The data used for this study can be found [here](https://www.kaggle.com/datasets/arashnic/fitbit). The data set examines personal fitness tracker data from thirty-three fitbit users from the one month period between April 12th 2016- May 12th 2016. The data examines information about daily activity, steps, heat rate, sleep habits, and calories burnt. 

### Cleaning/ Sorting Data 
```{r load packages}
library (tidyverse)
library(readr)
library(dplyr)
library(tidyr)
library(lubridate)
```

```{r load in data}
weight <- read.csv ("weightLogInfo_merged.csv")
activity <- read.csv('dailyActivity_merged.csv')
calories <- read.csv ("hourlyCalories_merged.csv")
intensity <- read.csv ("hourlyIntensities_merged.csv") 
sleep <- read.csv("sleepDay_merged.csv")
steps <- read.csv("hourlySteps_merged.csv")
heartrate <- read.csv ("heartrate_seconds_merged.csv")

#I used head to get a quick look at the data 
head(weight)
head(activity)
head(calories)
head(intensity)
head(sleep)
head(steps)
head(heartrate)

#From the head of these datasets: 
#1. ID and Date/Time are common in all the data frames. We can use these values to join our datasets. 
#2. We will need to convert the date field from AM/PM to a datetime format to be able to properly analyze our data 

#Used summary function to look for na values 
summary(weight)
summary(activity)
summary(calories)
summary(intensity)
summary(sleep)
summary(steps)
summary(heartrate)

#Na values were found in weight data, so I corrected this by setting those values to zero. 
weight[is.na (weight)]=0
summary(weight)

#Now I checked to see how many users logged data for each feature. 
length(unique(weight$Id))
length(unique(activity$Id))
length(unique(calories$Id))
length(unique(intensity$Id))
length(unique(sleep$Id))
length(unique(heartrate$Id))

#There are 33 participants in activity, calories, and intensities, 24 in sleep, and 8 in the weight data set. 8 is not significant enough to make any recommendations or conclusions based on this data set alone. 

#Next I checked for any duplicated values 
weight[duplicated(weight),]
activity[duplicated(activity),]
calories[duplicated(calories),]
intensity[duplicated(intensity),]
sleep[duplicated(sleep),]
heartrate[duplicated(heartrate),]

#A duplicate value was found in the sleep data set so I removed it so it would not affect my analysis. 
sleep %>% distinct(TotalSleepRecords, .keep_all = TRUE)
sleep[duplicated(sleep),]

#I then filtered the sleep data to make sure there weren’t any values that didn’t make sense. It wouldn’t make sense for there to be more time asleep than more time in bed. 
sleep %>% filter (TotalMinutesAsleep>TotalTimeInBed)

#Finally I updated the formatting of the time in the data sets from AM/PM time format to a 24 hour format. 
activity$ActivityDate = as.POSIXct(activity$ActivityDate,tz="UTC", format = "%m/%d/%Y")
sleep$SleepDay = as.POSIXct(sleep$SleepDay ,tz="UTC", format = "%m/%d/%Y %I:%M:%S %p")
```

Now that my data was cleaned and sorted, I went to Tableua to create my data visualizations.  

### Data Visualizations
<center><iframe src="https://public.tableau.com/views/CourseraCaseStudy2SleepHabits/Dashboard1?:showVizHome=no&:embed=true"width="650" height="860" frameborder="0"></iframe></center>


### Data limitations

1. There is no information about age or gender in the dataset provided. 
2. The dataset only has 33 participants, and only 8 users logged weight data. 
3. The data in the minute data sets were included in the daily and hourly data sets, so I only focused on the hourly data sets for my analysis

### Conclusion and Recommendations 
In summary: 
1. Number of steps is directly proportional to number of calories burnt
2. Number of steps is also proportional to hours asleep 
3. Users are more active during the first week and then interest seems to fade off 
4. Activity tracking is the feature most used 

After my data analysis of the fibit user data, I have the following recommendations on how Bellabeat app can improve their marketing strategies: 

Recommendations:
1. Activity tracking seems to be the most popular function for users. Encourage users to focus on this feature by including step challenges or total activity targets each user should hit per day. 
2. The more steps walked, the more calories burnt. Emphasis this to encourage users to walk more and burn more calories. 
3. Users seems to lose interest over time. They are very active with the device for the first week or so and then fade off in their usage. Encourage users to stay active on the device by providing reminders and daily incentives or challenges to keep users engaged. 
4. Highlight the link between more steps walked leading to longer sleep. Encourage users to improve their sleep by working out more. Provide advertisements around better sleep being driven by more exercise. 




