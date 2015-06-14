---
title: 'Reproducible Research: Peer Assessment 1'
output:
  html_document:
    keep_md: yes
---


## Loading and preprocessing the data

```{r}
dact<-read.csv("activity.csv")
```

## What is mean total number of steps taken per day?

```{r}
dailysteps<-aggregate(dact$steps, by=list(dact$date), FUN="sum")
hist(dailysteps$x)
mean(dailysteps$x,na.rm=TRUE)
median(dailysteps$x,na.rm=TRUE)
```

## What is the average daily activity pattern?

```{r}
intsteps<-aggregate(dact$steps, by=list(dact$interval), FUN="mean",na.rm=TRUE)
plot(intsteps$x,type="l")
mx<-which.max(intsteps$x)
intsteps[mx,1]
```

## Imputing missing values

```{r}
nas<-is.na(dact$steps)
table(nas)
ndact<-dact
for (i in 1:nrow(dact)){
if(is.na(dact[i,1])) ndact[i,1]<-intsteps[intsteps[,1]==dact[i,3],2]
}
dailynsteps<-aggregate(ndact$steps, by=list(ndact$date), FUN="sum")
hist(dailynsteps$x)
mean(dailynsteps$x,na.rm=TRUE)
median(dailynsteps$x,na.rm=TRUE)
```
Mean=10766.19
Median=10766.19

## Are there differences in activity patterns between weekdays and weekends?