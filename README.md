---
title: "Case Study - Bellabeat data analysis"
output:
  html_document:
    df_print: paged
    keep_md: true
  pdf_document: default
---

### About Bellabeat 

Bellabeat, founded in 2013 by Urška Sršen and Sando Mur, is a wellness tech company creating beautifully designed smart products for women. Their devices track activity, sleep, stress, and reproductive health, empowering women with health insights. Bellabeat markets extensively online and via traditional media channels.

###  Questions for Analysis

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?


### Business task

Identify growth opportunities and provide recommendations to enhance Bellabeat's marketing strategy by analyzing trends in smart device usage.

### Loading packages


``` r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library(lubridate)
library(dplyr)
library(ggplot2)
library(tidyr)
```

### Importing datasets


``` r
activity <- read.csv("dailyActivity_merged.csv")
calories <- read.csv("hourlyCalories_merged.csv")
intensities <- read.csv("hourlyIntensities_merged.csv")
sleep <- read.csv("sleepDay_merged.csv")
weight <- read.csv("weightLogInfo_merged.csv")
```

### Understanding datesets and types


``` r
head(activity)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["ActivityDate"],"name":[2],"type":["chr"],"align":["left"]},{"label":["TotalSteps"],"name":[3],"type":["int"],"align":["right"]},{"label":["TotalDistance"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["TrackerDistance"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["LoggedActivitiesDistance"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["VeryActiveDistance"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["ModeratelyActiveDistance"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["LightActiveDistance"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["SedentaryActiveDistance"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["VeryActiveMinutes"],"name":[11],"type":["int"],"align":["right"]},{"label":["FairlyActiveMinutes"],"name":[12],"type":["int"],"align":["right"]},{"label":["LightlyActiveMinutes"],"name":[13],"type":["int"],"align":["right"]},{"label":["SedentaryMinutes"],"name":[14],"type":["int"],"align":["right"]},{"label":["Calories"],"name":[15],"type":["int"],"align":["right"]}],"data":[{"1":"1503960366","2":"4/12/2016","3":"13162","4":"8.50","5":"8.50","6":"0","7":"1.88","8":"0.55","9":"6.06","10":"0","11":"25","12":"13","13":"328","14":"728","15":"1985","_rn_":"1"},{"1":"1503960366","2":"4/13/2016","3":"10735","4":"6.97","5":"6.97","6":"0","7":"1.57","8":"0.69","9":"4.71","10":"0","11":"21","12":"19","13":"217","14":"776","15":"1797","_rn_":"2"},{"1":"1503960366","2":"4/14/2016","3":"10460","4":"6.74","5":"6.74","6":"0","7":"2.44","8":"0.40","9":"3.91","10":"0","11":"30","12":"11","13":"181","14":"1218","15":"1776","_rn_":"3"},{"1":"1503960366","2":"4/15/2016","3":"9762","4":"6.28","5":"6.28","6":"0","7":"2.14","8":"1.26","9":"2.83","10":"0","11":"29","12":"34","13":"209","14":"726","15":"1745","_rn_":"4"},{"1":"1503960366","2":"4/16/2016","3":"12669","4":"8.16","5":"8.16","6":"0","7":"2.71","8":"0.41","9":"5.04","10":"0","11":"36","12":"10","13":"221","14":"773","15":"1863","_rn_":"5"},{"1":"1503960366","2":"4/17/2016","3":"9705","4":"6.48","5":"6.48","6":"0","7":"3.19","8":"0.78","9":"2.51","10":"0","11":"38","12":"20","13":"164","14":"539","15":"1728","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

``` r
head(calories)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["ActivityHour"],"name":[2],"type":["chr"],"align":["left"]},{"label":["Calories"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"1503960366","2":"4/12/2016 12:00:00 AM","3":"81","_rn_":"1"},{"1":"1503960366","2":"4/12/2016 1:00:00 AM","3":"61","_rn_":"2"},{"1":"1503960366","2":"4/12/2016 2:00:00 AM","3":"59","_rn_":"3"},{"1":"1503960366","2":"4/12/2016 3:00:00 AM","3":"47","_rn_":"4"},{"1":"1503960366","2":"4/12/2016 4:00:00 AM","3":"48","_rn_":"5"},{"1":"1503960366","2":"4/12/2016 5:00:00 AM","3":"48","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

``` r
head(activity)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["ActivityDate"],"name":[2],"type":["chr"],"align":["left"]},{"label":["TotalSteps"],"name":[3],"type":["int"],"align":["right"]},{"label":["TotalDistance"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["TrackerDistance"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["LoggedActivitiesDistance"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["VeryActiveDistance"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["ModeratelyActiveDistance"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["LightActiveDistance"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["SedentaryActiveDistance"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["VeryActiveMinutes"],"name":[11],"type":["int"],"align":["right"]},{"label":["FairlyActiveMinutes"],"name":[12],"type":["int"],"align":["right"]},{"label":["LightlyActiveMinutes"],"name":[13],"type":["int"],"align":["right"]},{"label":["SedentaryMinutes"],"name":[14],"type":["int"],"align":["right"]},{"label":["Calories"],"name":[15],"type":["int"],"align":["right"]}],"data":[{"1":"1503960366","2":"4/12/2016","3":"13162","4":"8.50","5":"8.50","6":"0","7":"1.88","8":"0.55","9":"6.06","10":"0","11":"25","12":"13","13":"328","14":"728","15":"1985","_rn_":"1"},{"1":"1503960366","2":"4/13/2016","3":"10735","4":"6.97","5":"6.97","6":"0","7":"1.57","8":"0.69","9":"4.71","10":"0","11":"21","12":"19","13":"217","14":"776","15":"1797","_rn_":"2"},{"1":"1503960366","2":"4/14/2016","3":"10460","4":"6.74","5":"6.74","6":"0","7":"2.44","8":"0.40","9":"3.91","10":"0","11":"30","12":"11","13":"181","14":"1218","15":"1776","_rn_":"3"},{"1":"1503960366","2":"4/15/2016","3":"9762","4":"6.28","5":"6.28","6":"0","7":"2.14","8":"1.26","9":"2.83","10":"0","11":"29","12":"34","13":"209","14":"726","15":"1745","_rn_":"4"},{"1":"1503960366","2":"4/16/2016","3":"12669","4":"8.16","5":"8.16","6":"0","7":"2.71","8":"0.41","9":"5.04","10":"0","11":"36","12":"10","13":"221","14":"773","15":"1863","_rn_":"5"},{"1":"1503960366","2":"4/17/2016","3":"9705","4":"6.48","5":"6.48","6":"0","7":"3.19","8":"0.78","9":"2.51","10":"0","11":"38","12":"20","13":"164","14":"539","15":"1728","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

``` r
head(sleep)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["SleepDay"],"name":[2],"type":["chr"],"align":["left"]},{"label":["TotalSleepRecords"],"name":[3],"type":["int"],"align":["right"]},{"label":["TotalMinutesAsleep"],"name":[4],"type":["int"],"align":["right"]},{"label":["TotalTimeInBed"],"name":[5],"type":["int"],"align":["right"]}],"data":[{"1":"1503960366","2":"4/12/2016 12:00:00 AM","3":"1","4":"327","5":"346","_rn_":"1"},{"1":"1503960366","2":"4/13/2016 12:00:00 AM","3":"2","4":"384","5":"407","_rn_":"2"},{"1":"1503960366","2":"4/15/2016 12:00:00 AM","3":"1","4":"412","5":"442","_rn_":"3"},{"1":"1503960366","2":"4/16/2016 12:00:00 AM","3":"2","4":"340","5":"367","_rn_":"4"},{"1":"1503960366","2":"4/17/2016 12:00:00 AM","3":"1","4":"700","5":"712","_rn_":"5"},{"1":"1503960366","2":"4/19/2016 12:00:00 AM","3":"1","4":"304","5":"320","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

I spotted some problems with the time stamp data. So before analysis, I need to convert it to date time format and split to date and time.

Intensities


``` r
intensities$ActivityHour=as.POSIXct(intensities$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
intensities$time <- format(intensities$ActivityHour, format = "%H:%M:%S")
intensities$date <- format(intensities$ActivityHour, format = "%m/%d/%y")
head(intensities)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["ActivityHour"],"name":[2],"type":["dttm"],"align":["right"]},{"label":["TotalIntensity"],"name":[3],"type":["int"],"align":["right"]},{"label":["AverageIntensity"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["time"],"name":[5],"type":["chr"],"align":["left"]},{"label":["date"],"name":[6],"type":["chr"],"align":["left"]}],"data":[{"1":"1503960366","2":"2016-04-12 00:00:00","3":"20","4":"0.333333","5":"00:00:00","6":"04/12/16","_rn_":"1"},{"1":"1503960366","2":"2016-04-12 01:00:00","3":"8","4":"0.133333","5":"01:00:00","6":"04/12/16","_rn_":"2"},{"1":"1503960366","2":"2016-04-12 02:00:00","3":"7","4":"0.116667","5":"02:00:00","6":"04/12/16","_rn_":"3"},{"1":"1503960366","2":"2016-04-12 03:00:00","3":"0","4":"0.000000","5":"03:00:00","6":"04/12/16","_rn_":"4"},{"1":"1503960366","2":"2016-04-12 04:00:00","3":"0","4":"0.000000","5":"04:00:00","6":"04/12/16","_rn_":"5"},{"1":"1503960366","2":"2016-04-12 05:00:00","3":"0","4":"0.000000","5":"05:00:00","6":"04/12/16","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Calories


``` r
calories$ActivityHour=as.POSIXct(calories$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
calories$time <- format(calories$ActivityHour, format = "%H:%M:%S")
calories$date <- format(calories$ActivityHour, format = "%m/%d/%y")
head(calories)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["ActivityHour"],"name":[2],"type":["dttm"],"align":["right"]},{"label":["Calories"],"name":[3],"type":["int"],"align":["right"]},{"label":["time"],"name":[4],"type":["chr"],"align":["left"]},{"label":["date"],"name":[5],"type":["chr"],"align":["left"]}],"data":[{"1":"1503960366","2":"2016-04-12 00:00:00","3":"81","4":"00:00:00","5":"04/12/16","_rn_":"1"},{"1":"1503960366","2":"2016-04-12 01:00:00","3":"61","4":"01:00:00","5":"04/12/16","_rn_":"2"},{"1":"1503960366","2":"2016-04-12 02:00:00","3":"59","4":"02:00:00","5":"04/12/16","_rn_":"3"},{"1":"1503960366","2":"2016-04-12 03:00:00","3":"47","4":"03:00:00","5":"04/12/16","_rn_":"4"},{"1":"1503960366","2":"2016-04-12 04:00:00","3":"48","4":"04:00:00","5":"04/12/16","_rn_":"5"},{"1":"1503960366","2":"2016-04-12 05:00:00","3":"48","4":"05:00:00","5":"04/12/16","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Activity


``` r
activity$ActivityDate=as.POSIXct(activity$ActivityDate, format="%m/%d/%Y", tz=Sys.timezone())
activity$date <- format(activity$ActivityDate, format = "%m/%d/%y")
head(activity)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["ActivityDate"],"name":[2],"type":["dttm"],"align":["right"]},{"label":["TotalSteps"],"name":[3],"type":["int"],"align":["right"]},{"label":["TotalDistance"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["TrackerDistance"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["LoggedActivitiesDistance"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["VeryActiveDistance"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["ModeratelyActiveDistance"],"name":[8],"type":["dbl"],"align":["right"]},{"label":["LightActiveDistance"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["SedentaryActiveDistance"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["VeryActiveMinutes"],"name":[11],"type":["int"],"align":["right"]},{"label":["FairlyActiveMinutes"],"name":[12],"type":["int"],"align":["right"]},{"label":["LightlyActiveMinutes"],"name":[13],"type":["int"],"align":["right"]},{"label":["SedentaryMinutes"],"name":[14],"type":["int"],"align":["right"]},{"label":["Calories"],"name":[15],"type":["int"],"align":["right"]},{"label":["date"],"name":[16],"type":["chr"],"align":["left"]}],"data":[{"1":"1503960366","2":"2016-04-12","3":"13162","4":"8.50","5":"8.50","6":"0","7":"1.88","8":"0.55","9":"6.06","10":"0","11":"25","12":"13","13":"328","14":"728","15":"1985","16":"04/12/16","_rn_":"1"},{"1":"1503960366","2":"2016-04-13","3":"10735","4":"6.97","5":"6.97","6":"0","7":"1.57","8":"0.69","9":"4.71","10":"0","11":"21","12":"19","13":"217","14":"776","15":"1797","16":"04/13/16","_rn_":"2"},{"1":"1503960366","2":"2016-04-14","3":"10460","4":"6.74","5":"6.74","6":"0","7":"2.44","8":"0.40","9":"3.91","10":"0","11":"30","12":"11","13":"181","14":"1218","15":"1776","16":"04/14/16","_rn_":"3"},{"1":"1503960366","2":"2016-04-15","3":"9762","4":"6.28","5":"6.28","6":"0","7":"2.14","8":"1.26","9":"2.83","10":"0","11":"29","12":"34","13":"209","14":"726","15":"1745","16":"04/15/16","_rn_":"4"},{"1":"1503960366","2":"2016-04-16","3":"12669","4":"8.16","5":"8.16","6":"0","7":"2.71","8":"0.41","9":"5.04","10":"0","11":"36","12":"10","13":"221","14":"773","15":"1863","16":"04/16/16","_rn_":"5"},{"1":"1503960366","2":"2016-04-17","3":"9705","4":"6.48","5":"6.48","6":"0","7":"3.19","8":"0.78","9":"2.51","10":"0","11":"38","12":"20","13":"164","14":"539","15":"1728","16":"04/17/16","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Sleep


``` r
sleep$SleepDay=as.POSIXct(sleep$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
sleep$date <- format(sleep$SleepDay, format = "%m/%d/%y")
head(sleep)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["SleepDay"],"name":[2],"type":["dttm"],"align":["right"]},{"label":["TotalSleepRecords"],"name":[3],"type":["int"],"align":["right"]},{"label":["TotalMinutesAsleep"],"name":[4],"type":["int"],"align":["right"]},{"label":["TotalTimeInBed"],"name":[5],"type":["int"],"align":["right"]},{"label":["date"],"name":[6],"type":["chr"],"align":["left"]}],"data":[{"1":"1503960366","2":"2016-04-12","3":"1","4":"327","5":"346","6":"04/12/16","_rn_":"1"},{"1":"1503960366","2":"2016-04-13","3":"2","4":"384","5":"407","6":"04/13/16","_rn_":"2"},{"1":"1503960366","2":"2016-04-15","3":"1","4":"412","5":"442","6":"04/15/16","_rn_":"3"},{"1":"1503960366","2":"2016-04-16","3":"2","4":"340","5":"367","6":"04/16/16","_rn_":"4"},{"1":"1503960366","2":"2016-04-17","3":"1","4":"700","5":"712","6":"04/17/16","_rn_":"5"},{"1":"1503960366","2":"2016-04-19","3":"1","4":"304","5":"320","6":"04/19/16","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Exploring and summarizing data


``` r
n_distinct(activity$Id)
```

```
## [1] 33
```

``` r
n_distinct(calories$Id)
```

```
## [1] 33
```

``` r
n_distinct(intensities$Id)
```

```
## [1] 33
```

``` r
n_distinct(sleep$Id)
```

```
## [1] 24
```

``` r
n_distinct(weight$Id)
```

```
## [1] 8
```


The activity, calories, and intensities data sets include 33 participants, while the sleep data set has 24 participants. However, the weight data set consists of only 8 participants, which is insufficient to draw any reliable conclusions or make informed recommendations based on this data.

### Summary statistics

Activity


``` r
activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>%
  summary()
```

```
##    TotalSteps    TotalDistance    SedentaryMinutes    Calories   
##  Min.   :    0   Min.   : 0.000   Min.   :   0.0   Min.   :   0  
##  1st Qu.: 3790   1st Qu.: 2.620   1st Qu.: 729.8   1st Qu.:1828  
##  Median : 7406   Median : 5.245   Median :1057.5   Median :2134  
##  Mean   : 7638   Mean   : 5.490   Mean   : 991.2   Mean   :2304  
##  3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.:1229.5   3rd Qu.:2793  
##  Max.   :36019   Max.   :28.030   Max.   :1440.0   Max.   :4900
```

Number of active minutes per category


``` r
activity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()
```

```
##  VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes
##  Min.   :  0.00    Min.   :  0.00      Min.   :  0.0       
##  1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:127.0       
##  Median :  4.00    Median :  6.00      Median :199.0       
##  Mean   : 21.16    Mean   : 13.56      Mean   :192.8       
##  3rd Qu.: 32.00    3rd Qu.: 19.00      3rd Qu.:264.0       
##  Max.   :210.00    Max.   :143.00      Max.   :518.0
```

Calories


``` r
calories %>%
  select(Calories) %>%
  summary()
```

```
##     Calories     
##  Min.   : 42.00  
##  1st Qu.: 63.00  
##  Median : 83.00  
##  Mean   : 97.39  
##  3rd Qu.:108.00  
##  Max.   :948.00
```

Sleep


``` r
sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()
```

```
##  TotalSleepRecords TotalMinutesAsleep TotalTimeInBed 
##  Min.   :1.000     Min.   : 58.0      Min.   : 61.0  
##  1st Qu.:1.000     1st Qu.:361.0      1st Qu.:403.0  
##  Median :1.000     Median :433.0      Median :463.0  
##  Mean   :1.119     Mean   :419.5      Mean   :458.6  
##  3rd Qu.:1.000     3rd Qu.:490.0      3rd Qu.:526.0  
##  Max.   :3.000     Max.   :796.0      Max.   :961.0
```

Weight


``` r
weight %>%
  select(WeightKg, BMI) %>%
  summary()
```

```
##     WeightKg           BMI       
##  Min.   : 52.60   Min.   :21.45  
##  1st Qu.: 61.40   1st Qu.:23.96  
##  Median : 62.50   Median :24.39  
##  Mean   : 72.04   Mean   :25.19  
##  3rd Qu.: 85.05   3rd Qu.:25.56  
##  Max.   :133.50   Max.   :47.54
```

Key findings from the summary include:

- The average sedentary time is 991 minutes (16 hours), which needs to be reduced.
- Most participants are lightly active.
- On average, participants sleep once for 7 hours.
- The average daily step count is 7,638, which is slightly below the threshold for health benefits. According to CDC research, taking 8,000 steps daily is linked to a 51% lower risk of all-cause mortality, while 12,000 steps daily reduces the risk by 65% compared to 4,000 steps.

### Merging Data


``` r
merged_data <- merge(sleep, activity, by=c('Id', 'date'))
head(merged_data)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Id"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[2],"type":["chr"],"align":["left"]},{"label":["SleepDay"],"name":[3],"type":["dttm"],"align":["right"]},{"label":["TotalSleepRecords"],"name":[4],"type":["int"],"align":["right"]},{"label":["TotalMinutesAsleep"],"name":[5],"type":["int"],"align":["right"]},{"label":["TotalTimeInBed"],"name":[6],"type":["int"],"align":["right"]},{"label":["ActivityDate"],"name":[7],"type":["dttm"],"align":["right"]},{"label":["TotalSteps"],"name":[8],"type":["int"],"align":["right"]},{"label":["TotalDistance"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["TrackerDistance"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["LoggedActivitiesDistance"],"name":[11],"type":["dbl"],"align":["right"]},{"label":["VeryActiveDistance"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["ModeratelyActiveDistance"],"name":[13],"type":["dbl"],"align":["right"]},{"label":["LightActiveDistance"],"name":[14],"type":["dbl"],"align":["right"]},{"label":["SedentaryActiveDistance"],"name":[15],"type":["dbl"],"align":["right"]},{"label":["VeryActiveMinutes"],"name":[16],"type":["int"],"align":["right"]},{"label":["FairlyActiveMinutes"],"name":[17],"type":["int"],"align":["right"]},{"label":["LightlyActiveMinutes"],"name":[18],"type":["int"],"align":["right"]},{"label":["SedentaryMinutes"],"name":[19],"type":["int"],"align":["right"]},{"label":["Calories"],"name":[20],"type":["int"],"align":["right"]}],"data":[{"1":"1503960366","2":"04/12/16","3":"2016-04-12","4":"1","5":"327","6":"346","7":"2016-04-12","8":"13162","9":"8.50","10":"8.50","11":"0","12":"1.88","13":"0.55","14":"6.06","15":"0","16":"25","17":"13","18":"328","19":"728","20":"1985","_rn_":"1"},{"1":"1503960366","2":"04/13/16","3":"2016-04-13","4":"2","5":"384","6":"407","7":"2016-04-13","8":"10735","9":"6.97","10":"6.97","11":"0","12":"1.57","13":"0.69","14":"4.71","15":"0","16":"21","17":"19","18":"217","19":"776","20":"1797","_rn_":"2"},{"1":"1503960366","2":"04/15/16","3":"2016-04-15","4":"1","5":"412","6":"442","7":"2016-04-15","8":"9762","9":"6.28","10":"6.28","11":"0","12":"2.14","13":"1.26","14":"2.83","15":"0","16":"29","17":"34","18":"209","19":"726","20":"1745","_rn_":"3"},{"1":"1503960366","2":"04/16/16","3":"2016-04-16","4":"2","5":"340","6":"367","7":"2016-04-16","8":"12669","9":"8.16","10":"8.16","11":"0","12":"2.71","13":"0.41","14":"5.04","15":"0","16":"36","17":"10","18":"221","19":"773","20":"1863","_rn_":"4"},{"1":"1503960366","2":"04/17/16","3":"2016-04-17","4":"1","5":"700","6":"712","7":"2016-04-17","8":"9705","9":"6.48","10":"6.48","11":"0","12":"3.19","13":"0.78","14":"2.51","15":"0","16":"38","17":"20","18":"164","19":"539","20":"1728","_rn_":"5"},{"1":"1503960366","2":"04/19/16","3":"2016-04-19","4":"1","5":"304","6":"320","7":"2016-04-19","8":"15506","9":"9.88","10":"9.88","11":"0","12":"3.53","13":"1.32","14":"5.03","15":"0","16":"50","17":"31","18":"264","19":"775","20":"2035","_rn_":"6"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Visualization


``` r
ggplot(data=activity, aes(x=TotalSteps, y=Calories)) + 
  geom_point(color='darkblue') + geom_smooth(color='darkcyan') + labs(title="Total Steps vs. Calories")
```

```
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

![](Bellabeat_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

I observe a positive correlation between total steps and calories, which is expected – increased activity leads to higher calorie expenditure.


``` r
ggplot(data=sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + 
  geom_point(color='darkblue')+ geom_smooth(color='darkcyan') + 
  labs(title="Total Minutes Asleep vs. Total Time in Bed")
```

```
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

![](Bellabeat_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

The relationship between total minutes asleep and total time in bed appears linear. To help Bellabeat users improve their sleep, we should consider sending notifications to remind them to go to bed.


``` r
int_new <- intensities %>%
  group_by(time) %>%
  drop_na() %>%
  summarise(mean_total_int = mean(TotalIntensity))

ggplot(data=int_new, aes(x=time, y=mean_total_int)) + geom_histogram(stat = "identity", fill='darkcyan') +
  theme(axis.text.x = element_text(angle = 90)) +
  labs(title="Average Total Intensity vs. Time")
```

```
## Warning in geom_histogram(stat = "identity", fill = "darkcyan"): Ignoring
## unknown parameters: `binwidth`, `bins`, and `pad`
```

![](Bellabeat_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

After visualizing total intensity hourly, I discovered that people are more active between 5 AM and 10 PM. The peak activity period is between 5 PM and 7 PM, likely due to people going to the gym or for a walk after work. We can use this time in the Bellabeat app to send reminders and motivate users to go for a run or walk.


``` r
ggplot(data=merged_data, aes(x=TotalMinutesAsleep, y=SedentaryMinutes)) + 
  geom_point(color='darkblue') + geom_smooth(color='darkcyan') +
  labs(title="Minutes Asleep vs. Sedentary Minutes")
```

```
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

![](Bellabeat_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

We can observe a clear inverse correlation between Sedentary Minutes and Sleep Time. To enhance sleep quality, the Bellabeat app could suggest reducing sedentary behavior.

### Recommendations for the business

Based on our data collection on activity, sleep, stress, and reproductive health, Bellabeat has successfully empowered women by providing valuable insights into their health and habits. Since its establishment in 2013, Bellabeat has rapidly grown and established itself as a leading tech-driven wellness company focused on women's health. 

Following an analysis of FitBit Fitness Tracker Data, I've identified key insights that can significantly impact Bellabeat's marketing strategy.

### Target audience

Our target audience includes individuals, particularly those working full-time jobs and spending extended periods at computers or in meetings, as indicated by our data on hourly intensity and sedentary time. These individuals engage in light activities for health maintenance, but they may benefit from guidance on enhancing their daily physical activity for greater health benefits. We aim our campaign towards all genders, assuming equal representation in our dataset.

### Key message for the Bellabeat online campaign

The core message of Bellabeat's online campaign is that our app goes beyond typical fitness applications. It serves as a supportive guide, empowering individuals—especially women—to achieve a balanced lifestyle by providing education and motivation through personalized daily recommendations.

### Ideas for the Bellabeat app

The average daily step count of 7,638 falls short of the CDC's recommended 10,000 steps for significant health benefits. Research shows that achieving 10,000 steps per day correlates with a 51% lower risk of all-cause mortality, with even greater benefits at 12,000 steps per day, associated with a 65% lower risk compared to 4,000 steps. Bellabeat can encourage users to reach at least 8,000 steps daily by highlighting these health advantages.

For weight loss goals, monitoring daily calorie intake is essential. Bellabeat can suggest low-calorie lunch and dinner options to assist users in managing their caloric consumption effectively.

To enhance sleep quality, Bellabeat could utilize app notifications to remind users of optimal bedtime routines.

The peak activity period between 5 pm and 7 pm suggests that users may engage in exercise after work. Bellabeat can capitalize on this window to motivate and remind users to engage in physical activities like running or walking.

As part of improving sleep, Bellabeat could recommend reducing sedentary time through its app.

