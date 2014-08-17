---
title: "Peer Assessment 1"
author: "Diego Racero"
date: "Saturday, August 16, 2014"
output: html_document
---

What is mean total number of steps taken per day?


```{r}
#reading the data fron de dataset
activity <- read.csv("C:/Users/dracero/Downloads/activity.csv")

#taking away NA values
complete<-activity[complete.cases(activity),]

#Calculating the missing values
emptysteps<-nrow(activity)-nrow(complete)

#ordering days decreasingly
avtivity<-activity[order(activity$date),]

#finding out the mean of steps
summary(complete)
mean(complete$steps)
median(complete$steps)
```
Now I plot the histogram

```{r}
hist(complete$steps, main="Steps")
```

What is the average daily activity pattern?

```{r}
library(ggplot2)
library(plyr)
data<-ddply(activity, .(date),summarise,mean=mean(steps))

qplot(1:length(mean),mean, data=data, geom="line", colour="Mean Activity")

```
The máximun number of steps taken in a given interval is:
```{r}
complete[max(complete$steps),]

```
I filled up the missing values with the median of stpes taken on average across all days. 

```{r}
#replacing in activity's NA values
missing<-activity[!complete.cases(activity),]
missing$steps<-37.38
filled<-rbind(missing,complete)

```
Now that I filled up the missing values I plot again the histogram and find the mean and median
```{r}
hist(filled$steps,main="Filled Table")
mean(filled$steps)
median(filled$steps)

```
Are there differences in activity patterns between weekdays and weekends?

```{r}
weekday<-activity[grepl("lunes",weekdays(as.Date(activity$date)),ignore.case=TRUE) | grepl("martes",weekdays(as.Date(activity$date)), ignore.case=TRUE)| grepl("miércoles",weekdays(as.Date(activity$date)), ignore.case=TRUE)| grepl("jueves",weekdays(as.Date(activity$date)), ignore.case=TRUE)| grepl("viernes",weekdays(as.Date(activity$date)), ignore.case=TRUE),]
weekend<-activity[grepl("sábado",weekdays(as.Date(activity$date)),ignore.case=TRUE) | grepl("domingo",weekdays(as.Date(activity$date)),ignore.case=TRUE),]


library(ggplot2)
p1 = qplot(interval,steps, data=weekend, geom="line", colour="Weekend")
p2 = qplot(interval,steps, data=weekday, geom="line", colour="weekday")
library(gridExtra)
grid.arrange(p1, p2, nrow=2, main = "Compare")

```

It is easy to note that in weekday there are more activity that in weekends.