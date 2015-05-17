#Reproducible research assignment


###Loading and preprocessing the data

1. Load the data (i.e. read.csv())
2. Process/transform the data (if necessary) into a format suitable for your analysis

```r
data <- read.csv("activity.csv")
datac <- data[complete.cases(data$steps) ,]
datav <- datac[ which(datac$steps > 0),]
```

###What is mean total number of steps taken per day?

- Ignore the missing values in the dataset

1. Calculate the total number of steps taken per day


```r
datam <- tapply(datav$steps,datav$date,mean)
datas <- tapply(datav$steps,datav$date,sum)
datac <- data.frame(date = rownames(datam), stepsmean = as.vector(datam) )
datasc <- data.frame(date = rownames(datas), stepstotal = as.vector(datas) )
```

2. Make a histogram of the total number of steps taken each day

```r
hist(datasc$stepstotal,main="Histogram of Total Steps per day",xlab="Total Steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

3. Calculate and report the mean and median of the total number of steps taken per day

```r
med <- median(datasc$stepstotal, na.rm = TRUE)
men <- mean(datasc$stepstotal, na.rm = TRUE)
```
####The mean is   : **1.0766189 &times; 10<sup>4</sup>**  
####The median is : **10765**

###What is the average daily activity pattern?


```r
datal <- data.frame(steps = datav$steps,interval = factor(datav$interval))
datalm <- tapply(datal$steps,datal$interval,mean)
datals <- data.frame(interval = rownames(datalm), stepsave = as.vector(datalm) )
datalst <- data.frame(interval = as.character(rownames(datalm)), stepsave = as.vector(datalm) )
dtest <- data.frame(interval = as.character(datals$interval),stepsave = datals$stepsave)
#convert factor to character
dtest[] <- lapply(datals, as.character)
```

1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
plot(dtest$interval, dtest$stepsave, type="l", xlab= "Interval", ylab= "Average Steps taken")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
mx <- dtest[which.max(as.numeric(dtest$stepsave)),1]
```

####The interval that contains the maximum number of steps is : **835**

###Imputing missing values

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
d <- sum(is.na(data))
```

####The total number of missing values is : **2304**
