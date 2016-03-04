# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
The code checks for activity.csv file. If it is not present then unzips the zip file ortherwise skips the process. The file is then read as csv and assigned to stepData.

```r
if(!file.exists('activity.csv')){
  unzip('activity.zip')
}

stepData <- read.csv("activity.csv",colClass=c('integer', 'Date', 'integer'))
```


## What is mean total number of steps taken per day?
The code aggregates the total number of steps takesn in a day. The values are displayed through a barplot. It also displays the mean and the median of the steps taken daily. 

```r
Dailysteps <- aggregate(steps ~ date, stepData, sum)
barplot(Dailysteps$steps,names.arg=Dailysteps$date,ylim=c(0, 20000), xlab="date", ylab="Total steps")
```

![plot](figure/Rplot1.png)

```r
Meanofsteps<-mean(Dailysteps$steps)
Meanofsteps
```

```
## [1] 10766.19
```

```r
Medianofsteps<-median(Dailysteps$steps)
Medianofsteps
```

```
## [1] 10765
```

## What is the average daily activity pattern?
The plot shows the average daily activy pattern in a 5-minute interval that, on average, contains the maximum number of steps.

```r
timeseriesplot<- aggregate(steps ~ interval,stepData,mean)

plot(timeseriesplot$steps,type = 'l',xlab ="Interval")
```

![plot](figure/Rplot2.png)

```r
maxstep<-timeseriesplot$interval[which.max(timeseriesplot$steps)]
maxstep
```

```
## [1] 835
```
## Imputing missing values
The missing value in the original dataset are imputed with mean values of the day. The barplot show the graph after the data has been imputed.

```r
sum(is.na(stepData$steps))
```

```
## [1] 2304
```

```r
ImputedData <- merge(stepData, Dailysteps, by = "date", suffixes = c("",".mean"))

NoValue <- is.na(ImputedData$steps)

ImputedData$steps[NoValue] <- ImputedData$steps.mean[NoValue]

ImputedData <- ImputedData[, c(1:3)]

DailystepsImp <- aggregate(steps ~ date, data = ImputedData, FUN = sum)

barplot(DailystepsImp$steps,names.arg=DailystepsImp$date,xlab="Date", ylab="Total steps")
```

![plot](figure/Rplot3.png)

```r
mean(DailystepsImp$steps)
```

```
## [1] 10766.19
```

```r
median(DailystepsImp$steps)
```

```
## [1] 10765
```

## Are there differences in activity patterns between weekdays and weekends?
Comparision of average number of step taken in 5 minute interval in Weekdays and Weekends are shown here.

```r
library(lattice)

stepData$DateType <-ifelse(as.POSIXlt(stepData$date)$wday %in% c(0,6), 'weekend', 'weekday')

timeseriesplot <- aggregate(steps ~ interval + DateType, stepData, mean)
xyplot(steps ~ interval | DateType, data=timeseriesplot, layout=c(2,1), type='l')
```

![plot](figure/Rplot4.png)
