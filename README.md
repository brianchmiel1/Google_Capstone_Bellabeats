# Bellabeats Data Analysis
## Google Data Analytics Capstone 

### Introduction

Bellabeat is a high-tech company that manufactures health-focused smart products. Developed products that  informs and inspires women around the
world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with
knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly
positioned itself as a tech-driven wellness company for women. Sršen (co-founder of Bellabeat) knows that an analysis of Bellabeat’s available consumer data would reveal more opportunities for growth. She has
asked the marketing analytics team to focus on a Bellabeat product and analyze smart device usage data in order to gain
insight into how people are already using their smart devices. Then, using this information, she would like high-level
recommendations for how these trends can inform Bellabeat marketing strategy.

### ASK

#### Stakeholders

1. Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer
2. Sando Mur: Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
3. Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and
reporting data that helps guide Bellabeat’s marketing strategy.
#### Products

1. Bellabeat App: Pprovides users with health data related to their activity, sleep, stress,
menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and
make healthy decisions. The Bellabeat app connects to their line of smart wellness products.
2. Leaf: Classic wellness tracker that can be worn as a bracelet, neckalce or clip. Tracks activity, sleep and stress.
3. Time: Combines the look of a watch with smart technology to track activity, sleep and stress.
4. Spring: Water bottle that tracks daily water intake using smart technology to make sure you are properly hydrated throughout the day.
5. Membership: Subscription based membership that gives 24/7 access to o fully personalized guidance on nutrition, activity, sleep, health and
beauty, and mindfulness based on their lifestyle and goals.

#### Business Task

The business task is to focus on a Bellabeat product and analyze smart device usage data in order to gain
insight into how people are already using their smart devices. Then, using this information, create high-level
recommendations for how these trends can inform Bellabeat marketing strategy.

#### Key Questions

* What are some trends in smart device usage?
* How could these trends apply to Bellabeat customers?
* How could these trends help influence Bellabeat marketing strategy?
* Are there any noticible gaps of usage that could be filled by marketing that feature? 
### PREPARE

#### Data Sources

FitBit Fitness Tracker Data which can be on [Kaggle]( https://www.kaggle.com/datasets/arashnic/fitbit) in 18 CSV files. The data set
contains personal fitness tracker from thirty fitbit users. Thirty eligible Fitbit users consented to the submission of
personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes
information about daily activity, steps, and heart rate that can be used to explore users’ habits.

##### Data Limitations

* The sample size of the data set is small since it only uses data from 30 users
* The data set is from 2016 and currently it is 2023, which makes this data 7 years old.

### PROCESS

#### Picking data files

I picked `dailyActivity_merged.csv ` and  `sleepDay_merged.csv` files because they included a good summary of the data for both their intended categories of activity and sleep. I included `weightLogInfo_merged` because they weight is a large piece of health data as most people are trying to lower it. 

#### Applications Used
I used RStudio for my data cleaning, analysis and visualisations as the data sets were not too large and I could do all of them in one application easily. 

### ANALYZE

#### Import and Explore

All of the R code can be found with this [link](https://github.com/brianchmiel1/Case-Study-2-How-Can-a-Wellness-Technology-Company-Play-It-Smart-/blob/main/r_script).

1.I loaded in the packages I will be using, tidyverse and janitor. 
2. Loaded and renamed the CSV files 

```
activity <- clean_names(read_csv("dailyActivity_merged.csv"))
sleep <- clean_names(read_csv("sleepDay_merged.csv"))
weight <- clean_names(read_csv("weightLogInfo_merged.csv"))
```
3. Checked to make sure the files were loaded correctly into RStudio
4. Converted ID to character, sleep_date to data and activity _date to date
```
activity <- activity %>% 
  mutate_at(vars(id), as.character) %>% 
  mutate_at(vars(activity_date), as.Date, format = "%m/%d/%y") %>% 
  rename("date"="activity_date")
sleep <- sleep %>% 
  mutate_at(vars(id), as.character) %>% 
  mutate_at(vars(sleep_day), as.Date, format = "%m/%d/%y") %>% 
  rename("date"="sleep_day")
weight <- weight %>% 
  mutate_at(vars(id), as.character) %>% 
  mutate_at(vars(date), as.Date, format = "%m/%d/%y") %>% 
  mutate_at(vars(log_id), as.character) %>% 
  mutate_at(vars(bmi), as.numeric)
```
5. Recheked data to make sure it correctly changed with the head() function.
6. Combined activity and sleep data
```
activity_and_sleep <- merge(activity, sleep, by = c("id","date"), all=TRUE)
```
7. Combine the new data frame with weight, add in day of the week column
```
all_data <- merge(weight, activity_and_sleep, by = c("id", "date"), all=TRUE) %>% 
  mutate(day_of_week = weekdays(as.Date(date, "m/%d/%y")))
```
8. Removed duplicates
```
all_data <- all_data[!duplicated(all_data), ]
```
9. Make the days of the week in order (better for visualisations)
```
all_data$day_of_week <- factor(all_data$day_of_week, levels = c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```


#### Summary Statistics

[Link](https://github.com/brianchmiel1/Case-Study-2-How-Can-a-Wellness-Technology-Company-Play-It-Smart-/blob/main/summary_statastics.png) to summary statistics. 

### SHARE

#### Data Visualizations

[Link](https://github.com/brianchmiel1/Case-Study-2-How-Can-a-Wellness-Technology-Company-Play-It-Smart-/tree/main/Data%20Visaualsations) to all of my graphs individually

#### Sedentary Minutes Per Day

The more active days of the week are Thursday, Friday and Saturday. 

#### Total Minutes Asleep vs Total Steps

Looking at this graph, there seems to be a negative relationship between sleep and total steps, this might be because that the more you sleep the less time you have to workout.


### ACT

* One of the big things that stuck out to me was that weight was only logged 67 times and only 41 of the times it was logged manually. I believe that Bellabeat is missing a large market that they could get into. I would suggest that they introduce a smart scale that will automattically log the users weight and other measuraments into the Bellabeat app so that users could track that data more easily.
* People are less sedentary on Thursday, Friday and Saturday. These are the days that people are going out and having fun. The least active days are Sundays and Mondays. I believe that this is most likely that people on Sunday are resting for the work week and then on Monday and Tuesdays people are more tired from work because they are just getting back into the swing of things. Bellabeat could add in a feature on their application as well as the Time product that gives a reminder so people to stand up and walk around when they have not for over 2 hours. This would help the sedentary minutes go down and more active minutes to go up. 

