# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
The code here checks for activity.csv file. If it is already there then skips the code otherwise unzips the file. The file is then read as csv and assigned to stepData.

```r
library(lattice)

if(!file.exists('activity.csv')){
  unzip('activity.zip')
}

stepData <- read.csv("activity.csv",colClass=c('integer', 'Date', 'integer'))
```


## What is mean total number of steps taken per day?
The code aggregates the total number of steps takesn in a day

```r
Dailysteps <- aggregate(steps ~ date, stepData, sum)
barplot(Dailysteps$steps,names.arg=Dailysteps$date,ylim=c(0, 20000), xlab="date", ylab="Total steps",)
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)
## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
